---
layout: post
title: "Raspberry Pi Backup Script"
date: 2020-05-19 17:11:45 +0000
categories: 2020
tags: shell raspberrypi sysadmin
excerpt: A BASH shell script to backup only certain directories. Used on my RPi devices
---


## gist: [Raspberry Pi backup script](https://gist.github.com/jftuga/7559ee580bfb283033a210e559ade77e)

**File:** make_backup.sh

```
#!/bin/bash

# Side note:
# for better performance append to /etc/sysctl.conf (and then reboot)
# vm.vfs_cache_pressure = 50
# vm.swappiness = 10
# vm.dirty_writeback_centisecs = 1500

# location of backups
BASE="/data/backups"

# what to back up
FOLDERS="etc home root"

DOCKER="/var/lib/docker" # note the beginning slash
if [ -e ${DOCKER} ] ; then
	FOLDERS+=" ${DOCKER:1}"
fi

UNIFI="/var/lib/unifi"
if [ -e ${UNIFI} ] ; then
	FOLDERS+=" ${UNIFI:1}"
fi

LOCALBIN="/usr/local/bin"
if [ -e ${LOCALBIN} ] ; then
	FOLDERS+=" ${LOCALBIN:1}"
fi

NOW=`date +%Y%m%d.%H%M%S`
DIR="${BASE}/backup--${NOW}"
PKG="${DIR}/installed_packages.txt"
FILES="${DIR}/all_files.txt"
ARCHIVE="${DIR}/${HOSTNAME}--${NOW}.tar"
LOG="${DIR}/status.log"
OLDLOG="${DIR}/old.log"
DFLOG="${DIR}/df.txt"

umask 077
if [ ! -e ${BASE} ] ; then
	mkdir -m 0700 ${BASE}
fi
mkdir ${DIR}

# create a list of user installed packages
zgrep -hE '^(Start-Date:|Commandline:)' $(ls -tr /var/log/apt/history.log* ) | egrep -v 'aptdaemon|upgrade' | egrep -B1 '^Commandline:' >| ${PKG}

# create a file system list
# https://github.com/jftuga/fstat
find / 2> /dev/null | /usr/local/bin/fstat -er '/sys/|^/proc/|^/run/|^/dev/' -sn -t -c 2> ${LOG} | xz -9 - > ${FILES}.xz 2> ${LOG}

# remove backup directories older then RETAIN days
RETAIN=18
echo >> ${OLDLOG} 2>&1
echo "Removing older backup directories (if any)" >> ${OLDLOG} 2>&1
find ${BASE} -maxdepth 1 -name 'backup--*' -type d -mtime +${RETAIN} >> ${OLDLOG} 2>&1
find ${BASE} -maxdepth 1 -name 'backup--*' -type d -mtime +${RETAIN} -exec rm -rf "{}" \; >> ${OLDLOG} 2>&1
echo >> ${OLDLOG} 2>&1

# create a backup archive
cd / && tar -c -v '--exclude=.cache/*' '--exclude=*/cache/*' '--exclude=*/.cache/*' '--exclude=*/.local/*' '--exclude=var/lib/docker/devicemapper/devicemapper/*' -f ${ARCHIVE} ${FOLDERS} >> ${LOG} 2>&1
ls -l ${ARCHIVE} >> ${LOG} 2>&1

# use this when not on a Raspberry Pi
#THREADS=`grep -c '^processor' /proc/cpuinfo`
THREADS=1

xz -9 -T${THREADS} ${ARCHIVE} >> ${LOG} 2>&1
ls -l ${ARCHIVE}.xz >> ${LOG} 2>&1

df -Th -x tmpfs -x devtmpfs -x vfat > ${DFLOG}

```


