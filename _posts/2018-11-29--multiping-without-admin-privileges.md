---
layout: post
title: "multiping without admin privileges"
date: 2018-11-29 15:00:33 +0000
categories: c# networking sysadmin
---


## gist: [multiping without admin privileges](https://gist.github.com/jftuga/482be32ad050deb91512839b57642654)

**File:** mping.cs

```
/*
 * Mulithreaded ping that does not require admin privileges
 * Give IPs and/or hostnames on command-line
 * The ping timeout is hard-coded to 100 ms, but can be changed by updating: pingTimeOut
 */

using System;
using System.Net;
using System.Runtime.InteropServices;
using System.Text.RegularExpressions;
using System.Threading;
using System.Threading.Tasks;

namespace mping
{
    public class IcmpPing
    {
        [StructLayout(LayoutKind.Sequential, CharSet = CharSet.Ansi)]
        private struct ICMP_OPTIONS
        {
            public byte Ttl;
            public byte Tos;
            public byte Flags;
            public byte OptionsSize;
            public IntPtr OptionsData;
        }

        [StructLayout(LayoutKind.Sequential, CharSet = CharSet.Ansi)]
        private struct ICMP_ECHO_REPLY
        {
            public int Address;
            public int Status;
            public int RoundTripTime;
            public short DataSize;
            public short Reserved;
            public IntPtr DataPtr;
            public ICMP_OPTIONS Options;
            [MarshalAs(UnmanagedType.ByValTStr, SizeConst = 250)]
            public string Data;
        }


        [DllImport("icmp.dll", SetLastError = true)]
        private static extern IntPtr IcmpCreateFile();
        [DllImport("icmp.dll", SetLastError = true)]
        private static extern bool IcmpCloseHandle(IntPtr handle);
        [DllImport("icmp.dll", SetLastError = true)]
        private static extern int IcmpSendEcho(IntPtr icmpHandle, int destinationAddress, string requestData, short requestSize, ref ICMP_OPTIONS requestOptions, ref ICMP_ECHO_REPLY replyBuffer, int replySize, int timeout);

        public static Regex ip_re = new Regex(@"^((25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\.){3}(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)$", RegexOptions.Compiled);
        private static int errorCount = 0;
        private const int pingTimeOut = 100;

        // ping IP address using IcmpSendEcho, https://www.pinvoke.net/default.aspx/icmp.icmpsendecho
        public static bool pingIP(IPAddress ip)
        {
            IntPtr icmpHandle = IcmpCreateFile();
            ICMP_OPTIONS icmpOptions = new ICMP_OPTIONS();
            icmpOptions.Ttl = 255;
            ICMP_ECHO_REPLY icmpReply = new ICMP_ECHO_REPLY();
            string sData = "x";

            int iReplies = IcmpSendEcho(icmpHandle, BitConverter.ToInt32(ip.GetAddressBytes(), 0), sData, (short)sData.Length, ref icmpOptions, ref icmpReply, Marshal.SizeOf(icmpReply), pingTimeOut);
            IcmpCloseHandle(icmpHandle);
            if (icmpReply.Status == 0)
                return true;
            return false;
        }

        public static bool testHost(string host)
        {
            IPHostEntry hostEntry;
            IPAddress ipaddress = null;
            String arrow = "";

            try
            {
                // determine if IP address or dns name is the current host
                MatchCollection matches = ip_re.Matches(host);
                if (matches.Count > 0) // ip address given
                {
                    try
                    {
                        ipaddress = System.Net.IPAddress.Parse(host);
                    }
                    catch (Exception e)
                    {
                        Console.WriteLine("{0}: {1}", host, e.Message);
                        return false;
                    }
                }
                else // hostname given
                {
                    hostEntry = Dns.GetHostEntry(host);
                    if (hostEntry.AddressList.Length == 0)
                    {
                        Console.WriteLine("Error: Unable to resolve {0}", host);
                        return false;
                    }
                    ipaddress = hostEntry.AddressList[0];
                }

                bool result = pingIP(ipaddress);
                arrow = (result) ? "" : " <<-------------------";
                Console.WriteLine("{0}\t{1}\t{2}\t{3}", host, ipaddress.ToString(), result, arrow);
                if (!result)
                {
                    return false;
                }
            }
            catch (Exception e)
            {
                Console.WriteLine("{0}: {1}", host, e.Message);
                return false;
            }
            return true;
        }

        #region Parallel_Loop
        static int Main(string[] args)
        {
            if( 0 == args.Length )
            {
                Console.WriteLine("mping:");
                Console.WriteLine("Give (multiple) IPs and/or hostnames on command-line.");
                return 1;

            }

            Parallel.For(0, args.Length, i =>
            {
                if( false == testHost(args[i]) )
                {
                    Interlocked.Add(ref errorCount, 1);
                }
            });

            return errorCount;
        }
        #endregion
    }
}

```

---

