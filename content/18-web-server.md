# Web Server (Apache HTTPD)

Apache HTTPD is the original and widely used web server on RHEL systems.

## Installation and Configuration

Install the Apache HTTPD package:

```bash
yum install -y httpd
```

## Configuration Files

The main configuration file is `/etc/httpd/conf/httpd.conf`. Additional snap-in configuration files can be placed under `/etc/httpd/conf.d/`.

Ensure the `Listen 80` directive is present in the configuration to serve HTTP traffic on port 80.

## Starting and Enabling the Service

Enable and start the HTTPD service:

```bash
systemctl enable --now httpd
```

Restart the service after configuration changes:

```bash
systemctl restart httpd
```

## Basic Virtual Host Setup

Create a simple test page to verify the server is working:

```bash
echo "Hello world" > /var/www/html/index.html
```

## Verifying the Service

Use `curl` to confirm the web server responds:

```bash
curl localhost
```

Make sure port 80 is open in the firewall for external access.
