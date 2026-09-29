# AWS EC2 Static Website Deployment

A hands-on Cloud/DevOps project demonstrating how to deploy a static website to an **AWS EC2 instance** using **Amazon Linux 2023, Nginx, Git, and SSH**.

The website source code is hosted on GitHub and deployed to an EC2 server, where Nginx serves the static files to the internet.

## Architecture

```text
Local Machine
     │
     │ git push
     ▼
   GitHub
     │
     │ git clone / git pull
     ▼
 AWS EC2
(Amazon Linux 2023)
     │
     │ Nginx
     ▼
/usr/share/nginx/html
     │
     ▼
  Internet
     │
     ▼
 Web Browser
```

## Technologies Used

* **AWS EC2** — Virtual server hosting the website
* **Amazon Linux 2023** — Operating system
* **Nginx** — Web server
* **Git & GitHub** — Source-code management
* **SSH** — Secure remote access
* **firewalld** — Linux firewall
* **HTTP/HTTPS** — Web traffic

## Project Structure

```text
website-deployment-on-aws/
│
├── portfolio-website/
│   ├── index.html
│   ├── style.css
│   ├── images/
│   └── ...
│
├── docs/
│   ├── architecture.md
│   ├── deployment.md
│   ├── troubleshooting.md
│   └── lessons-learned.md
│
├── screenshots/
│   ├── ec2-instance.png
│   ├── firewalld.png
│   ├── nginx-status.png
│   ├── ssh-connection.png
│   ├── terminal-deployment.png
│   └── website-lve.png
│
└── README.md
```

## Deployment Process

1. Created an AWS EC2 instance running Amazon Linux 2023.
2. Configured the EC2 Security Group for SSH and web traffic.
3. Connected to the server using SSH.
4. Installed Git and Nginx.
5. Confirmed that Nginx was running.
6. Cloned the website repository from GitHub.
7. Copied the website files to the Nginx web root.
8. Tested the Nginx configuration.
9. Verified the website locally using `curl`.
10. Accessed the website through the EC2 public IP.

## Useful Commands

Check the operating system:

```bash
cat /etc/os-release
```

Install packages:

```bash
sudo yum install git nginx -y
```

Check Nginx:

```bash
sudo systemctl status nginx
```

Test Nginx configuration:

```bash
sudo nginx -t
```

Restart Nginx:

```bash
sudo systemctl restart nginx
```

View the website files:

```bash
ls -la /usr/share/nginx/html
```

Test the website from the server:

```bash
curl http://localhost
```

## Key Learning Outcomes

This project helped me develop practical experience with:

* Launching and configuring an AWS EC2 instance
* Connecting to Linux servers using SSH
* Working with Amazon Linux 2023
* Using Git and GitHub for deployment
* Installing and managing Nginx
* Understanding Linux web-server directories
* Configuring AWS Security Groups
* Working with `firewalld`
* Troubleshooting Linux and Nginx issues
* Deploying and updating a static website

## Lessons Learned

One of the biggest lessons from this project was that Linux distributions can differ significantly in their package management, firewall configuration, and filesystem conventions.

Coming from Ubuntu, I initially tried to use `apt` and expected the Nginx web root to be `/var/www/`. On Amazon Linux 2023, I learned to work with `dnf`/`yum`, `firewalld`, and the Nginx web root at `/usr/share/nginx/html`.

I also learned that SSH commands depend on the location of the private key file, so understanding the current working directory and using the correct path to the `.pem` file is important.

## Future Improvements

Potential improvements to this project include:

* Configure HTTPS using SSL/TLS
* Connect the deployment to a custom domain
* Automate deployments with GitHub Actions
* Add Nginx configuration for the website
* Implement a more automated deployment process

## Repository

**GitHub:** `samachillesx/website-deployment-on-aws`
