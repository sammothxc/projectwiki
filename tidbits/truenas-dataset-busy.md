---
title: TrueNAS: Dataset Busy
description: 
published: true
date: 2026-01-19T05:29:44.066Z
tags: 
editor: markdown
dateCreated: 2026-01-19T05:29:44.066Z
---

# TrueNAS: 'Dataset Busy' when deleting

Figure out which process(es) are using dataset:
`grep "$YOUR_DATASET_NAME" /proc/*/mounts`

List processes and filter by the id(s) from last step:
`ps -aux | grep $PROC_ID`
