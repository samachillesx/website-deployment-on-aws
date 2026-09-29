# Troubleshooting

This document records issues encountered while deploying the static website to Amazon Linux 2023 on AWS EC2.

## 1. UFW vs firewalld

### Problem

I initially approached the server as if it were an Ubuntu system and expected to use UFW for firewall management.

### What I learned

Ubuntu commonly uses UFW as a simplified firewall management tool, while Amazon Linux 2023 uses `firewalld`.

Check the firewall:

```bash
sudo systemctl status firewalld
```

Check firewall configuration:

```bash
sudo firewall-cmd --list-all
```

For AWS EC2, inbound access is also controlled by the **EC2 Security Group**, so both the cloud-level network rules and the server configuration need to be understood.

### Lesson

Do not assume that administration commands from one Linux distribution will be identical on another.

---

## 2. Using the wrong package manager

### Problem

I initially kept using Ubuntu's:

```bash
apt
```

### Solution

Amazon Linux supports:

```bash
dnf
```

and also provides:

```bash
yum
```

For example:

```bash
sudo dnf install nginx -y
```

or:

```bash
sudo yum install nginx -y
```

### Lesson

Always identify the operating system before installing packages.

```bash
cat /etc/os-release
```

This helps determine which package-management tools and system conventions to use.

---

## 3. SSH private key location

### Problem

I initially struggled to connect to the EC2 instance because the `.pem` private key was stored in a different directory from my current terminal location.

### Solution

Navigate to the directory containing the key:

```bash
cd /path/to/keypem
```

Then connect:

```bash
ssh -i your-key.pem ec2-user@YOUR_EC2_PUBLIC_IP
```

Alternatively, provide the complete path to the key:

```bash
ssh -i /path/to/keypem/your-key.pem ec2-user@YOUR_EC2_PUBLIC_IP
```

### Lesson

The terminal's current working directory matters when using relative file paths.

---

## 4. Locating the Nginx web root

### Problem

I initially expected the Nginx website directory to be:

```text
/var/www/
```

because that is a common location on Ubuntu.

### Solution

On this Amazon Linux/Nginx setup, the website files were located at:

```text
/usr/share/nginx/html/
```

I verified the directory with:

```bash
ls -la /usr/share/nginx/html/
```

The website was then copied into the Nginx web root:

```bash
sudo cp -r portfolio-website/* /usr/share/nginx/html/
```

### Lesson

Filesystem locations can differ between Linux distributions and server configurations. Rather than assuming a path, locate and verify the actual Nginx configuration and document root.

---

## 5. Testing the Website Locally

If the website does not load in a browser, first test it from the EC2 server:

```bash
curl http://localhost
```

If this returns the website HTML, Nginx is serving the site locally.

If the site works locally but not from the internet, check:

* EC2 Security Group
* Port 80
* EC2 public IP
* Nginx status
* AWS networking configuration
* Server firewall configuration
