# Configuring Apache web server

**The Apache HTTP Server** (“httpd”) is one of the most commonly used web servers on Linux systems and is often a key component of the LAMP stack. In this guide, I will demonstrate the steps required to configure Apache as a web server.

## Step 1: Installing required packages(Apache2, mysql-server, PHP)

- Installing Apache2 (Web server), MySQL(used as DataBase):

```bash
sudo apt install apache2 mysql-server
```

- Install PHP packages

```bash
sudo apt install php php-cli php-mysql libapache2-mod-php -y
```

## Step 2: Create the root directory

- This directory, named student_reg, will hold the website files

```bash
sudo mkdir -p /var/www/html/student_reg
```

## Step 3: Set permissions for the web directory

To make sure Apache can read and serve the website files.

```bash
sudo chown -R $USER:www-data /var/www/html/student_reg
sudo chmod -R 755 /var/www/html/student_reg
```

## Step 4: Add website files

Place the website files, such as index.php, style.css, and other necessary files, inside:

```bash
/var/www/html/student_reg/
```

## Step 5: Create an Apache Virtual Host

Each virtual host is like a separate "website" configuration inside the server. It tells Apache how to handle HTTP and HTTPS requests for a specific domain.

```bash
sudo nano /etc/apache2/sites-available/student_reg.conf
```

## Step 6: Enable the site and reload Apache

```bash
sudo a2ensite student_reg.conf
sudo systemctl reload apache2 

```

## Step 7: Map the domain locally

Inside the `/etc/hosts` Add the following line to resolve `student_reg.local`

```bash
127.0.0.1 student_reg.local

```
Apache is now configured to serve a website using a virtual host, and the website "student_reg.local" is accessible locally.
