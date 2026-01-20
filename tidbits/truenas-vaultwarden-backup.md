---
title: TrueNAS: Vaultwarden Backup
description: 
published: true
date: 2026-01-20T05:30:52.193Z
tags: tidbit, truenas
editor: markdown
dateCreated: 2026-01-20T05:04:32.375Z
---

# TrueNAS: Vaultwarden Backup

## Backup

Vaultwarden Postgres 17 TrueNAS app stores data in the path `/<HOST_PATH>/postgres_data/17/docker` with perms `drwx------ 19 netdata docker 26 Jan 17 04:05 docker`

This location can be backed up with the command ```sudo tar cf /<BACKUP_LOCATION>/vaultwarden-`date +%Y%m%d%H%M%S`.tar -C /mnt/.ix-apps/app_mounts/vaultwarden/postgres_data/17/docker```

The database can be dumped with the command `sudo docker exec -it ix-vaultwarden-postgres-1 pg_dump -U vaultwarden -F t | gzip >/<BACKUP_LOCATION>/vault-$(date +%Y-%m-%d).tar.gz`

> Thanks [coolcash](https://forums.truenas.com/t/vaultwarden-postgres-backup/34619/6)!

## Restore

#### Easy Way

Spin up a new instance of Vaultwarden, making sure to use the same database password, then shut the instance down

Navigate to the new instance's `docker` folder, and delete it. Copy the original backup `docker` folder to its spot. Chown it with correct perms with `sudo chown -R netdata:docker docker`

Start up the new instance, and log in normally

#### Hard Way

Let's hope I never have to find out how to use `pg_restore`