# Step-by-Step Setup for Apache and Nginx on RHEL 9

This guide shows how to manually set up Apache and Nginx web servers on a RHEL 9 system.

---

## 1. Apache Setup

### 1.1 Install Apache

```bash
sudo dnf install httpd -y
```

### 1.2 Start and Enable Apache Service

```bash
sudo systemctl start httpd
sudo systemctl enable httpd
sudo systemctl status httpd
```

### 1.3 Configure Firewall

```bash
sudo firewall-cmd --permanent --add-service=http
sudo firewall-cmd --permanent --add-service=https
sudo firewall-cmd --reload
```

### 1.4 Test Default Page

```bash
curl http://localhost
```

Open a browser and go to `http://<your-server-ip>` to see the Apache default page.

### 1.5 Create a Custom Virtual Host

```bash
sudo mkdir -p /var/www/example
echo "<h1>Hello from Apache!</h1>" | sudo tee /var/www/example/index.html
sudo nano /etc/httpd/conf.d/example.com.conf
```

Example Apache config (`example.com.conf`):

```apache
<VirtualHost *:80>
    ServerName example.com
    DocumentRoot /var/www/example
</VirtualHost>
```

Restart Apache to apply changes:

```bash
sudo systemctl restart httpd
```

---

## 2. Nginx Setup

### 2.1 Install Nginx

```bash
sudo dnf install nginx -y
```

### 2.2 Configure Nginx on Port 8080

```bash
sudo nano /etc/nginx/nginx.conf
```

Change `listen 80;` to `listen 8080;` to avoid conflict with Apache.

### 2.3 Create Test Website

```bash
sudo mkdir -p /usr/share/nginx/example
echo "<h1>Hello from Nginx!</h1>" | sudo tee /usr/share/nginx/example/index.html
sudo nano /etc/nginx/conf.d/example.com.conf
```

Example Nginx config (`example.com.conf`):

```nginx
server {
    listen 8080;
    server_name localhost;
    root /usr/share/nginx/example;
    index index.html;
}
```

### 2.4 Start and Enable Nginx

```bash
sudo systemctl start nginx
sudo systemctl enable nginx
sudo systemctl status nginx
```

### 2.5 Optional Firewall for Custom Port

```bash
sudo firewall-cmd --permanent --add-port=8080/tcp
sudo firewall-cmd --reload
```

### 2.6 Test Nginx Page

Open a browser and go to `http://<your-server-ip>:8080` to see the Nginx test page.

---

## 3. Tips and Troubleshooting

* If Apache or Nginx fails to start, check for port conflicts.
* Use `systemctl status <service>` and `journalctl -xe` to see errors.
* Make sure firewall rules allow the ports you configured.
* Restart services after changing configuration files.
* Take screenshots of each step for documentation.

