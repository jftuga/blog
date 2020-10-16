---
layout: post
title: "Retrieve Active Directory Contacts Belonging to a Group"
date: 2020-07-02 13:33:52 +0000
category: 2020
tags: c# active-directory
excerpt: Surprisingly, Microsoft does not provide an easy way via PowerShell map Active Directory Contacts to Groups.  C# to the rescue!
---


## gist: [Retrieve Active Directory Contacts Belonging to a Group](https://gist.github.com/jftuga/b50c31d3b77795a2397975da7cd3b222)

**File:** active_directory_contacts.cs

```
/*

Given an AD group name, return all of the AD Contacts that belong to that group

Return a list of strings in this format:
CN Name|Email Address

*/

using System.DirectoryServices;

private List<string> GetGroupContacts(String ad_group_name)
{
    DirectoryEntry rootDSE = new DirectoryEntry("LDAP://RootDSE");
    string domainContext = rootDSE.Properties["defaultNamingContext"].Value as string;
    DirectoryEntry searchRoot = new DirectoryEntry("LDAP://" + domainContext);
    String cn_group, look_for;
    int j;
    List<string> contacts = new List<string>();

    using (DirectorySearcher searcher = new DirectorySearcher(searchRoot, "(&(objectCategory=person)(objectClass=contact))", new string[] { "cn", "mail", "memberof" }, SearchScope.Subtree))
    {
        foreach (SearchResult result in searcher.FindAll())
        {
            foreach (string member in result.Properties["memberof"])
            {
                for (j = 0; j < result.Properties["memberof"].Count; j++)
                {
                    cn_group = result.Properties["memberof"][j].ToString();
                    look_for = "CN=" + ad_group_name + ",";
                    Console.WriteLine("cn_group: {0}", cn_group);
                    if (0 == cn_group.IndexOf(look_for))
                    {
                        Console.WriteLine("   Match found: {0}", result.Properties["cn"][0]);
                        contacts.Add(result.Properties["cn"][0] + "|" + result.Properties["mail"][0]);
                    }
                }
            }
        }
    }
    return contacts;
}

```

---

