# Architecture

## Overview

In this project, I deployed a static website to an **AWS EC2 instance** running **Amazon Linux 2023**.

Nginx is used as the web server and serves the website files stored in its web root directory.

## Architecture Diagram

```text
                    ┌─────────────────┐
                    │  Local Machine  │
                    │                 │
                    │ portfolio-      │
                    │ website         │
                    └────────┬────────┘
                             │
                         git push
                             │
                             ▼
                    ┌─────────────────┐
                    │     GitHub      │
                    │                 │
                    │ Website Source  │
                    └────────┬────────┘
                             │
                      git clone/pull
                             │
                             ▼
              ┌──────────────────────────┐
              │        AWS EC2           │
              │    Amazon Linux 2023     │
              │                          │
              │  ┌────────────────────┐  │
              │  │       Nginx        │  │
              │  │                    │  │
              │  │ /usr/share/nginx/  │  │
              │  │ html/              │  │
              │  └─────────┬──────────┘  │
              └────────────┼─────────────┘
                           │
                         HTTP
                           │
                           ▼
                    ┌─────────────────┐
                    │   Web Browser   │
                    └─────────────────┘
```

## Components

### AWS EC2

Provides the virtual Linux server used to host the website.

### Amazon Linux 2023

The operating system running on the EC2 instance.

The project uses Amazon Linux-specific tools and conventions rather than Ubuntu-specific ones.

### GitHub

Stores the website source code and provides the repository used to retrieve the application files on the EC2 server.

### Nginx

Nginx acts as the web server.

The website files are served from:

```text
/usr/share/nginx/html/
```

### SSH

SSH provides secure remote access to the EC2 instance.

The EC2 private key (`.pem`) is used for authentication.

### AWS Security Group

The EC2 Security Group controls inbound network traffic to the instance.

The project requires:

| Traffic | Port | Purpose                |
| ------- | ---: | ---------------------- |
| SSH     |   22 | Remote administration  |
| HTTP    |   80 | Website traffic        |
| HTTPS   |  443 | Secure website traffic |

> HTTPS is planned as an improvement to the initial deployment.

## Request Flow

When a visitor accesses the EC2 public IP:

```text
Browser
   │
   │ HTTP request
   ▼
AWS EC2
   │
   ▼
Nginx
   │
   ▼
/usr/share/nginx/html/index.html
   │
   ▼
Website returned to browser
```

## Security Considerations

* SSH access should be restricted to trusted IP addresses where practical.
* The `.pem` private key must never be committed to GitHub.
* AWS Security Groups should only expose required ports.
* HTTPS should be configured for production use.
* Linux file permissions should prevent unnecessary write access to web files.
