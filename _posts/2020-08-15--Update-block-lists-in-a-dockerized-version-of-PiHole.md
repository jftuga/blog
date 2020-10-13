---
layout: post
title: "Update block lists in a dockerized version of PiHole"
date: 2020-08-15 11:40:47 +0000
categories: shell docker pihole
---


## [Update block lists in a dockerized version of PiHole](https://gist.github.com/jftuga/48e008acf7c5e24dbca002311d88cea7)

**File:** update_pihole_block_lists.sh

```
#!/bin/bash

# Update block lists in a dockerized version of PiHole
# the log file should be: update_pihole_block_lists.log
#
# run this script from cron each Saturday morning with this contab entry:
# 00 3 * * 6 /root/update_pihole_block_lists.sh

IMAGE=pihole
LENGTH=12
LOG="${0%.*}.log"
CONTAINER=$(docker ps --format "{{.ID}}" --filter name=${IMAGE})
if [ "${#CONTAINER}" -ne "${LENGTH}" ] ; then
        echo "No running container for image: ${IMAGE}"
        exit 1
fi

echo >> ${LOG}
echo >> ${LOG}
echo >> ${LOG}
echo "==============================================================" >> ${LOG}
date >> ${LOG}
echo "==============================================================" >> ${LOG}
docker exec -t ${CONTAINER} pihole updateGravity >> ${LOG} 2>&1

```

