---
title: Hosting a built ExpressJS backend
date: 2025-07-29
tags:
  - hosting
  - remote
  - linux
  - back-end
  - expressJS
category: Linux Server Hosting
status: in_corso
author: Te3sk
description: Instructions to host an ExpressJS backend already built locally from a tar archive on a Linux Remote Server
---
# Hosting a built ExpressJS backend
The first step is to [[Web Development/ExpressJS/Build|build the ExpressJS project]] and get a [[Web Development/ExpressJS/Build#Compress the project|tar.gz archive]].
# Premises
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
To start, we have to copy our [[Web Development/ExpressJS/Build#Compress the project|local tar.gz archive]] in the `srv` directory of the server. If the server can contain more than one project or system, is a **good practice** to create a subdirectory in `srv` and move the archive there.
To copy the archive we run the following command from the folder containing the archive:
```bash
scp [archive name].tar.gz [user]@[remote server ip] 
```
# Extract the project
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
# Install PM2
[PM2](https://pm2.keymetrics.io/docs/usage/quick-start/) is a production process manager for Node.js applications with a built-in load balancer. It allows you to keep applications forever alive, reload them without downtime, and facilitate common system administration tasks. Essentially, PM2 enables you to run your Node.js applications in the background, ensure they automatically restart if they crash, and distribute the load across multiple CPU cores to maximize performance and reliability in a production environment.

First we have to install PM2 on the remote server:
```bash
npm install pm2@latest -g
```
# Start the server with PM2
In the `dist` directory there should be the `server.js` file, which acts as the entry point for the server:
```bash
pm2 start dist/server.js --name [project name]
```
and update the environment variables
```bash
pm2 restart [project name] --update-env
```
# Test the server
Now that we've started the server, we can check if it works. First we check the status:
```bash
pm2 status
```
It should be something like this:
```
┌────┬────────────────────┬──────────┬──────┬───────────┬──────────┬──────────┐
│ id │ name               │ mode     │ ↺    │ status    │ cpu      │ memory   │
├────┼────────────────────┼──────────┼──────┼───────────┼──────────┼──────────┤
│ 0  │ trovapulizie-api   │ fork     │ 45   │ online    │ 0%       │ 97.3mb   │
└────┴────────────────────┴──────────┴──────┴───────────┴──────────┴──────────┘
```
If we receive an error from PM2 Status, we can look at the logs to figure out where the problem is. It's good practice to look at them even if there are no apparent errors from the Status, as there may be some minor errors or warnings that are good to be aware of.
```bash
pm2 logs
```
