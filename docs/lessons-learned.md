# Lessons Learned

This project gave me practical experience deploying a static website to an AWS EC2 instance running Amazon Linux 2023.

## 1. Linux Distributions Have Different Conventions

Coming from Ubuntu, I initially expected Amazon Linux to work in exactly the same way.

I learned that Linux distributions can differ in:

* Package managers
* Firewall tools
* Default users
* Filesystem locations
* Service configuration

This made identifying the operating system an important first step.

```bash
cat /etc/os-release
```

## 2. Package Management

I was used to Ubuntu's `apt` command and initially kept trying to use it.

Amazon Linux uses `dnf` and also supports `yum`.

Example:

```bash
sudo dnf install nginx git -y
```

This reinforced the importance of understanding the operating system before running administration commands.

## 3. Firewall Management

I learned the difference between Ubuntu's commonly used UFW workflow and Amazon Linux's `firewalld`.

I also learned that an AWS EC2 server has another important network-control layer: the **EC2 Security Group**.

This helped me understand the difference between:

```text
AWS Security Group
        ↓
Cloud-level network access

firewalld
        ↓
Host/Server-level firewall

Nginx
        ↓
Web-server configuration
```

## 4. SSH and File Paths

One of the issues I encountered was locating my EC2 `.pem` key.

I initially struggled because the terminal was not in the directory containing the key.

I learned to either navigate to the key directory:

```bash
cd /path/to/keypem
```

or provide the complete path:

```bash
ssh -i /path/to/keypem/key.pem ec2-user@SERVER_IP
```

This improved my understanding of Linux working directories and relative versus absolute paths.

## 5. Nginx Web Root

I initially expected the website files to be under:

```text
/var/www/
```

because of my previous Ubuntu experience.

For this setup, Nginx served files from:

```text
/usr/share/nginx/html/
```

This taught me not to assume that directory structures are identical across Linux distributions.

## 6. Troubleshooting Method

The project also reinforced a structured approach to troubleshooting.

Instead of immediately changing multiple things, I learned to check each layer independently:

```text
AWS
 ↓
Security Group
 ↓
EC2
 ↓
Linux
 ↓
Firewall
 ↓
Nginx
 ↓
Website Files
 ↓
Browser
```

For example:

```bash
systemctl status nginx
```

checks the service,

```bash
nginx -t
```

checks the Nginx configuration,

and:

```bash
curl http://localhost
```

checks whether the web server is actually returning content.

## 7. Overall Takeaway

The biggest lesson from this project was that deploying a website is not just about copying files to a server.

It requires understanding the relationship between the cloud infrastructure, operating system, networking, firewall, web server, filesystem, and application files.

This project gave me a practical foundation for moving from basic Linux administration into AWS and DevOps workflows.
