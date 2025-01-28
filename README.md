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



