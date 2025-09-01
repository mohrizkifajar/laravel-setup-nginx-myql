# Deploy Laravel Nginx & Mysql

## 1. SSH Configuration

### Create new user & add to sudo group

```
adduser your_username

usermod -aG sudo your_username
```

### Disable root login
```
nano /etc/ssh/sshd_config
```

### SSH authentication for Github
```
cd .ssh/

ssh-keygen -t ecdsa -b 521

cat id_github.pub
# copy the output and paste in on Github

eval "$(ssh-agent -s)"

ssh-add ~/.ssh/id_github

ssh -T git@github.com
```

## 2. Install PHP, Composer, Node JS, MySQL

### Install PHP & extensions
```
sudo apt update
sudo add-apt-repository ppa:ondrej/php
sudo apt install php8.2-cli
sudo apt install -y php8.2-common php8.2-bcmath php8.2-mbstring php8.2-xml php8.2-curl php8.2-gd php8.2-zip php8.2-mysql php8.2-fpm

php -v
```

### Install Composer
```
sudo apt install composer

composer --version
```

### Install NVM
```
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.3/install.sh | bash
# OR
wget -qO- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.3/install.sh | bash

nvm -v

nvm install --lts
```

### Setup MySQL
```
sudo apt install mysql-server

sudo mysql_secure_instalation

sudo mysql

CREATE USER 'username'@'localhost' IDENTIFIED BY 'password';
GRANT ALL PRIVILEGES ON database.* TO 'username'@'localhost';
FLUSH PRIVILEGES;
EXIT;
```

## 3. Web Server Configuration

### Install Nginx
```
sudo apt install nginx
```

### Prepare project folder
```
cd /var/www/

sudo chown -R $USER html/

cd html/project/

sudo chown -R $USER:www-data .

sudo find . -type f -exec chmod 664 {} \;

sudo find . -type d -exec chmod 775 {} \;

sudo chgrp -R www-data storage\ bootstrap\cache\

sudo chmod -R ug+rwx storage\ bootstrap\cache\
```

### Nginx virtual host configuration
```
cd /etc/nginx/sites-available/

sudo nano example.com
# Copy config from Laravel docs

sudo ln -s /etc/nginx/sites-available/example.com /etc/nginx/sites-enabled/

sudo nginx -t

sudo systemctl nginx restart

sudo apt install certbot python3-cerbot-nginx

sudo certbot --nginx -d example.com
```

## Links

https://github.com/glennraya/server-setup/blob/main/Commands.txt
https://github.com/glennraya/laravel-deploy-yml/blob/main/deploy.yml