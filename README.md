# LEMP-STACK-guide
This is a simple step by step guide for configure a LEMP STACK on Fedora 4 or Ubuntu

## STEP 1 - Update System

Fedora:  
```bash
sudo dnf update
```

Ubuntu:  
```bash
sudo apt update
```

## STEP 2 - Instalation and Configuration of NGINX

#### Fedora: 
```bash
sudo dnf install nginx
```

#### Ubuntu:  
```bash
sudo apt install nginx
```

### Enable nginx service

```bash
sudo systemctl enable nginx  
sudo systemctl start nginx  
sudo systemctl stauts nginx
```
### Enable services on firewall

#### Fedora 
```bash
sudo firewall-cmd --permanent --add-service=http  
sudo firewall-cmd --permanent --add-service=https  
```
```bash
sudo firewall-cmd --permanent --add-port=80/tcp
sudo firewall-cmd --permanent --add-port=443/tcp  
```
```bash
sudo firewall-cmd --reload

sudo firewall-cmd --list-all
```

#### Ubuntu
```bash
sudo ufw allow http  
sudo ufw allow https  
```


## STEP 3 Installation and Configuration of MariaDB

#### Fedora  
```bash
sudo dnf install mariadb-server mariadb -y
```

#### Ubuntu
```bash
sudo apt install mariadb-server mariadb -y
```

### Enable MariaDB service

```bash
sudo systmectl enable mariadb
sudo systemctl start mariadb

sudo systemctl status mariadb
```

### Configure MariaDB

```bash
sudo mysql_secure_installation
```

> Answer the questions for the configuration of the databse with the next refererences

* Set root password? > yes. Set a password for the root user
* Remove anonymous users? > yes
* Disallow root login? > yes
* Remove test database? > yes
* Reload privilege tables? > yes

### Conect to database
```bash 
mysql -u root -p
```



## STEP 4 - Instalation and Configuration of PHP

### Install PHP and php-fpm

#### Fedora
```bash
sudo dnf install php 
```
> If the packages php-fpm and php-mysqlnd dosen't been installed, run the next comands

```bash
sudo dnf install php-fpm
```
```bash
sudo dnf install php-mysqlnd
```
#### Ubuntu
```bash
sudo apt install php 
```
> If the packages php-fpm and php-mysqlnd dosen't been installed, run the next comands

```bash
sudo apt install php-fpm
```
```bash
sudo apt install php-mysqlnd
```
#### Configure php-fpm for nginx
Edit the file ***www.conf*** on the next directory
```bash
sudo nano /etc/php-fpm.d/www.conf
```

And change the next lines of code  
#### User and group
```ini
user = nginx
group = nginx
```
#### Listen socket
```ini
listen = /run/php-fpm/www.sock
```

#### Enable PHP service

``` bash
sudo systemctl enable php-fpm
sudo systemctl start php-fpm

sudo systemctl status php-fpm
```

## Cofiguration NGINX for PHP

Create a file on the next directory:

```bash
sudo nano /etc/nginx/conf.d/default.conf
```

And paste the next text:

```nginx
server {
    listen 80;
    server_name localhost;

    root /usr/share/nginx/html;
    index index.php index.html index.htm;

    location / {
        try_files $uri $uri/ =404;
    }

    location ~ \.php$ {
        include fastcgi_params;
        fastcgi_pass unix:/run/php-fpm/www.sock;
        fastcgi_index index.php;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
    }

    location ~ /\.ht {
        deny all;
    }
}
```

>Save and close the file with ***ctrl + o*** for save an ***ctrl + x*** for exit

### Restar Nginx
```bash 
sudo systemctl restart nginx
```

### Test that php are conected to nginx

Create a ***info.php*** on the next directory
```bash
sudo nano /usr/share/nginx/html/info.php
```

In the file add the next lines of code
```php
<?php
phpinfo();
?>
```
>Save and close the file with ***ctrl + o*** for save an ***ctrl + x*** for exit

>Now you can acces to http://localhost/info.php on your browser
