# SSL Setup Guide with mkcert

## Step 1: Install mkcert

  

- Enable mkcert to add its certificates to Firefox’s NSS trust store (Network Security Services)

```bash
sudo apt install libnss3-tools -y
```

- Download the latest mkcert binary

```bash
sudo wget https://github.com/FiloSottile/mkcert/releases/latest/download/mkcert-v1.4.4-linux-amd64 -O mkcert
```

- Make it executable

```bash
chmod +x mkcert
```

- Move it to the system path, so I can use it globally

```bash
sudo mv mkcert /usr/local/bin/
```

## Step 2: Set up the local CA (Certificate Authority)

- Create a local CA that will be automatically trusted by Ubuntu and Firefox, since libnss3 is installed

```bash
mkcert -install
```

## Step 3: Generate a certificate

 ******

- student_reg is a custom domain added to /etc/hosts/

```bash
mkcert student_reg.local <- This will create two files, as shown below in the image
```

![Certificate files.png](SSL%20Setup%20Guide%20with%20mkcert%2021bc5a6a6b21808294beec58e45ceb47/Certificate_files.png)

- student_reg.local-key.pem → Private key
- student_reg.local.pem → Certificate

## Step 4: Configure Apache to use the SSL certificate

- Create a new Apache virtual host for SSL and add the following configurations

```bash
** Host Creation **
sudo nano /etc/apache2/sites-available/student_reg-ssl.conf/
** Configurations **
								<VirtualHost *:443>
										    ServerName student_reg.local
										    DocumentRoot /var/www/html/student_reg
										
										    SSLEngine on
										    SSLCertificateFile /home/doe/student_reg.local.pem <- Use the actual path to where certificate files are stored
										    SSLCertificateKeyFile /home/doe/student_reg.local-key.pem
										
										    <Directory /var/www/html/student_reg>
										        AllowOverride All
										        Require all granted
										    </Directory>
										
										    ErrorLog ${APACHE_LOG_DIR}/student_reg_ssl_error.log <- To log erors
										    CustomLog ${APACHE_LOG_DIR}/student_reg_ssl_access.log combined
								</VirtualHost>
				
```

## Step 5: Enable  SSL and site config

- This enables the SSL module and the new HTTPS site

```bash
sudo a2enmod ssl
sudo a2ensite student_reg-ssl.conf
```

## Step 6: Redirect HTTP to HTTPS

- Redirect all HTTP traffic to HTTPS to enforce encrypted communication.

```bash

sudo nano /etc/apaeche2/sites-available/student_reg.conf

** Add the following **
							<VirtualHost *:80>
									    ServerName student_reg.local <- HTTP site to be redirected
									    Redirect permanent / https://student_reg.local/
							</VirtualHost>

```

## Step 7: Reload and verify

- Reload Apache for changes to take effect

```bash
sudo systemctl reload apache2
```

- When the website is visited, it will now have a padlock in front of the URL. Images below can show before the SSL certificate and after.

![Screenshot 2025-06-19 at 20.20.53.png](SSL%20Setup%20Guide%20with%20mkcert%2021bc5a6a6b21808294beec58e45ceb47/Screenshot_2025-06-19_at_20.20.53.png)

![Screenshot 2025-06-23 at 13.47.33.png](SSL%20Setup%20Guide%20with%20mkcert%2021bc5a6a6b21808294beec58e45ceb47/Screenshot_2025-06-23_at_13.47.33.png)
