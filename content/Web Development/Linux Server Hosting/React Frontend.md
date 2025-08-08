---
title: Hosting a built React frontend
date: 2025-08-08
tags:
  - front-end
  - hosting
  - linux
  - react
  - remote
  - ip
category: Linux Server Hosting
status: in_corso
author: Te3sk
description: Instructions to host a React frontend already built locally from a tar archive on a Linux Remote Server
---
The first step is to [[Web Development/React/Build|build the project]] and get a [[Web Development/React/Build#Compress the project|tar.gz archive]].
## Premises
Our linux remote server should have a structure like this:
```bash
$ tree

├── bin -> usr/bin
├── boot
├── cdrom
├── dev
├── etc
├── home
├── lib -> usr/lib
├── lib32 -> usr/lib32
├── lib64 -> usr/lib64
├── libx32 -> usr/libx32
├── lost+found
├── media
├── mnt
├── opt
├── proc
├── root
├── run
├── sbin -> usr/sbin
├── snap
├── srv
├── swap.img
├── sys
├── tmp
├── usr
└── var
```
The folder we're interested in is `srv`, which stands for `services`, and is used to contain data served by the system, such as websites, FTP, database data, or web applications. It separates service data from other system data or temporary files.
# Transfer the file
To start, we have to copy our [[Web Development/React/Build#Compress the project|local tar.gz archive]] in the `srv` directory of the server. If the server can contain more than one project or system, is a **good practice** to create a subdirectory in `srv` and move the archive there.
To copy the archive we run the following command from the folder containing the archive:
```bash
scp [archive name].tar.gz [user]@[remote server ip] 
```
# Extract the project and deploy
Once this is done, we can access to the remote server whit the **ssh protocol**:
```bash
ssh [user]@[remote server ip]
```
and move into the folder and extract the project in a specific folder:
```bash
cd /srv/
mkdir [project]
cd [project]
tar -xzvf ../archives/[archive name].tar.gz
```
Now you should have a ready-to-deploy folder in the `srv` directory of your remote server. To deploy it, just [[NGINX#Configuration File|configure NGINX system]].