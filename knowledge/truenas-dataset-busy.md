---
title: TrueNAS: Dataset Busy
description: 
published: true
date: 2026-02-05T05:14:10.926Z
tags: truenas, kb
editor: markdown
dateCreated: 2026-01-19T05:29:44.066Z
---

# TrueNAS: 'Dataset Busy' when deleting

Figure out which process(es) are using dataset:
```bash
grep "$YOUR_DATASET_NAME" /proc/*/mounts
```

List processes and filter by the id(s) from last step:
```bash
ps -aux | grep $PROC_ID
```

> Thanks to [a8es](https://www.truenas.com/community/threads/unable-to-delete-datasets-busy.108347/post-795396)!