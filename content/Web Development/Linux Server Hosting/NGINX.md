---
title: NGINX setup
date: 2025-07-29
tags:
  - linux
  - remote
  - hosting
  - network
  - ip
  - domain
category: Linux Server Hosting
status: completed
author: Te3sk
description: How to setup nginx to host a complete site made by both frontend and backend
---
# Introduction
## What is Nginx?

**Nginx** (pronounced "engine-x") is a powerful, open-source web server that can also be used as a reverse proxy, load balancer, mail proxy, and HTTP cache. It's renowned for its high performance, stability, rich feature set, simple configuration, and low resource consumption.
## What is Nginx Used For?

Nginx is widely adopted for serving static content and dynamic HTTP content, acting as a crucial component in modern web infrastructure. Its primary uses include:
- **Web Server:** Efficiently serving static files like HTML, CSS, JavaScript, and images.
- **Reverse Proxy:** Forwarding client requests to backend servers (like your Express.js application) and returning the servers' responses to the client. This adds a layer of abstraction and can enhance security.
- **Load Balancer:** Distributing incoming network traffic across multiple backend servers to ensure no single server is overloaded, thereby improving responsiveness and availability.
- **API Gateway:** Managing API requests, often handling authentication, rate limiting, and routing to various microservices.
## How Nginx is Structured
Nginx operates with an event-driven, asynchronous, non-blocking architecture. This design allows it to handle thousands of concurrent connections with minimal overhead. At its core, Nginx typically runs a **master process** that reads configuration and manages **worker processes**. The worker processes handle the actual request processing, efficiently managing connections through a non-blocking approach. Its configuration is primarily done through simple, human-readable text files, allowing for highly flexible and customizable setups.
## Why Nginx is Essential
Nginx is essential for high-performance web applications because it can handle a large number of concurrent connections more efficiently than many traditional web servers. By offloading tasks like serving static files, SSL termination, and load balancing from your application server (like Node.js with Express), Nginx frees up your application to focus solely on processing dynamic requests. This separation of concerns improves the overall stability, scalability, and security of your web service, leading to a faster and more reliable experience for users.
# Premises
This section outlines the steps to install Nginx on a remote Ubuntu Linux server. This process typically involves updating your package list, installing **Nginx**, and then performing basic service management.
First connect to your remote linux server with
```bash
ssh [user]@[remote server ip]
```
It's good practice to update your local package index to ensure you get the latest versions of packages:
```bash
sudo apt update
```
## Installation and first setup
Now, install Nginx using the `apt` package manager:
```bash
sudo apt install nginx
```
During the installation, you might be prompted to confirm. Type `Y` and press Enter.
You can check the firewall status with:
```bash
systemctl status nginx
```
You should see an output indicating `active (running)`.

# Configuration File
An Nginx configuration file defines how your web server handles incoming requests, serves static files, and proxies API calls to backend services. It is typically structured as one or more **server blocks**, each listening on specific ports and domains, with multiple **location blocks** describing how to respond to requests under particular URL paths.

Once installed `nginx` on the remote server, you can access the directory with
```bash
cd /etc/nginx
```
and create the configuration file:
```bash
nano sites-available/[project name]
```
## Web Server Hosting Setup
The frontend section handles serving the static assets of your web application (e.g., React, Vue, Angular build files). It typically includes:
- **`server` block**: Defines the domain(s) and port(s) Nginx listens on (e.g., HTTP port 80, HTTPS port 443).
- **`root` directive**: Path on the server’s filesystem where the frontend's static files are located (usually the build output directory like `dist/`).
- **`index` directive**: The default file to serve when a directory is requested (usually `index.html`).
- **`location /` block**: Defines how to serve requests to the root URL and SPA routes. Uses `try_files` to serve static files if present, or fallback to `index.html` for SPA routing.
- **SSL configuration** (if using HTTPS): Paths to SSL certificate and key, plus SSL options for secure communication.
```nginx
server {
    listen 443 ssl http2;
    server_name example.com www.example.com;

    root /path/to/frontend/dist;
    index index.html;

    ssl_certificate /path/to/fullchain.pem;
    ssl_certificate_key /path/to/privkey.pem;
    include /path/to/options-ssl-nginx.conf;
    ssl_dhparam /path/to/ssl-dhparams.pem;

    location / {
        try_files $uri $uri/ /index.html;
    }
}
```
## API Gateway Setup
The backend section proxies API requests from the frontend to your backend server. Key points:
- **`location /api/` block**: Matches any request starting with `/api/` and forwards it to the backend service (often running on localhost with a different port).
- **`proxy_pass` directive**: The backend URL where the API server is listening (e.g., `http://localhost:3000/`).
- **`proxy_set_header` directives**: Forward important headers (like `Host`, client IP, and protocol) to maintain correct request context and support WebSocket upgrades.
- **`proxy_http_version`**: Usually set to `1.1` to support features like chunked transfer encoding and keep-alive connections.
- **`proxy_cache_bypass`**: Helps avoid caching for WebSocket upgrades or certain requests.
```nginx
location /api/ {
    proxy_pass http://localhost:3000/;
    proxy_http_version 1.1;

    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection 'upgrade';
    proxy_set_header Host $host;

    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header X-Forwarded-Proto $scheme;

    proxy_cache_bypass $http_upgrade;
}
```
## HTTP to HTTPS
A separate `server` block listens on port 80 and redirects all requests to HTTPS:
```nginx
server {
    listen 80;
    server_name example.com www.example.com;

    return 301 https://$host$request_uri;
}
```
This improves security by forcing encrypted connections.
# Activate the configuration
## Enable the site
Now you have to create a symbolic link in the folder `sites-enabled` to enable the site:
```bash
ln -s /etc/nginx/sites-available/[project name] /etc/nginx/sites-enabled/
```

After that, check the semantic correctness of the [[#Configuration File|configuration file]]:
```bash
sudo nginx -t
```
If the file is correct you will see something like this:
```
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful
```
## Reload nginx
To apply the new configuration, restart the nginx system:
```bash
sudo systemctl reload nginx
```
Check that nginx is running correctly:
```bash
sudo systemctl status nginx
```
And if there is some issue check the logs
```bash
sudo tail -n 50 /var/log/nginx/error.log
```
# Troubleshooting
## Installation and setup
### Nginx Service Not Running

If Nginx isn't active after installation, you can try starting and enabling it:
```bash
sudo systemctl start nginx
sudo systemctl enable nginx
sudo systemctl status nginx
```
If it still fails, check the error logs for more details:
```bash
sudo journalctl -xe
```
### "Connection Refused" or Browser Shows Nothing
This usually indicates a firewall blocking access or Nginx not listening on the expected port. 
* **Firewall:** Ensure **UFW** (or any other firewall) allows traffic on ports 80 (HTTP) and/or 443 (HTTPS). 
```bash
sudo ufw allow 'Nginx Full'
sudo ufw reload
sudo ufw status 
```
If you're using a cloud provider, check their security group or network ACL settings as well. 

* **Nginx Configuration:** Verify Nginx is configured to listen on the correct ports. The default configuration usually handles this, but a custom setup might have issues. Check `/etc/nginx/sites-available/default` or your custom configuration files.
### "403 Forbidden" Error
A "403 Forbidden" error typically means Nginx is running, but it doesn't have permission to access the files it's trying to serve.
* **File Permissions:** Ensure the user Nginx runs as (usually `www-data`) has read access to the webroot directory and its contents.
```bash
ls -l /var/www/html # Default Nginx webroot
```
You might need to adjust permissions:
```bash
sudo chmod -R 755 /var/www/html
sudo chown -R www-data:www-data /var/www/html
```

* **Incorrect Root Directory:** Double-check your Nginx server block configuration (`/etc/nginx/sites-available/default` or custom config) to ensure the `root` directive points to the correct directory where your website files are located.
### Incorrect Nginx Configuration (`nginx -t` fails)

Before restarting Nginx after making configuration changes, always test the syntax of your configuration files.
```bash
sudo nginx -t
```
This command will tell you if there are any syntax errors and where they are located. If it reports an error, correct it in the specified file, then re-run `sudo nginx -t` until it confirms `syntax is ok` and `test is successful`.
### Port Already in Use 
If another service is already using port 80 or 443, Nginx won't be able to start. 
* **Check for Conflicts:** 
```bash 
sudo netstat -tulpn | grep :80 sudo netstat -tulpn | grep :443 
``` 
This will show you which process is listening on those ports. You'll need to stop the conflicting service or configure Nginx to listen on a different port. Apache is a common culprit if you've had it installed previously.
### General Tips
* **Check Nginx Error Logs:** For more detailed information on what's going wrong, always consult the Nginx error logs: 
```bash
sudo tail -f /var/log/nginx/error.log
```
* **Restart Nginx:** After making any configuration changes, always restart Nginx for them to take effect:
```bash
sudo systemctl restart nginx
```
By systematically checking these points, you should be able to diagnose and resolve most common Nginx installation and configuration issues on your Ubuntu server.
