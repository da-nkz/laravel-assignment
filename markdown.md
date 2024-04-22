#!/bin/bash
#update your linux system
sudo apt update

#upgrade your linux repository
sudo apt upgrade -y

#install your apache webserver
sudo apt install apache2 -y

#add the php ondrej repository
sudo add-apt-repository ppa:ondrej/php

#update your repository again
sudo apt update

#upgrade your repository
sudo apt upgrade -y

# install php8.2

sudo apt install php8.2 -y

#install some of those php dependencies that are needed for laravel to work
sudo apt install php8.2-curl php8.2-dom php8.2-mbstring php8.2-xml php8.2-mysql zip unzip -y

#enable rewrite
sudo a2enmod rewrite

#restart your apache server
sudo systemctl restart apache2

#install mysql
sudo apt install mysql -y
sudo systemctl start mysql

#change directory in the bin directory
cd /usr/bin

#install composer
sudo curl -sS https://getcomposer.org/installer | sudo php -y

#move the content of the default composer.phar
sudo mv composer.phar composer

#change directory in /var/www directory so we can clone laravel repo
cd /var/www/
sudo git clone https://github.com/da-nkz/laravel.git
sudo chown -R root laravel
cd laravel/

#install composer autoloader
sudo composer install --optimize-autoloader --no-dev
sudo composer update

#copy the content of the default env file to .env
sudo cp .env.example .env
sudo chown -R www-data storage
sudo chown -R www-data bootstrap/cache

#create new default laravel homepage
cd /etc/apache2/sites-available/
sudo touch laravel.conf
sudo cp /home/vagrant/latest.conf laravel.conf
sudo a2ensite laravel.conf
sudo systemctl restart apache2

#edit database
sudo mysql -uroot -e "CREATE DATABASE Altschool;"
sudo mysql -uroot -e "CREATE USER 'Daniel'@'192.168.53.13' IDENTIFIED BY 'Sapphire';"
sudo mysql -uroot -e "GRANT ALL PRIVILEGES ON Altschool.\* TO 'Daniel'@'192.168.53.13';"
sudo sed -i 's/DB_CONNECTION=mysql/DB_CONNECTION=mysql/g' /var/www/laravel/.env
sudo sed -i 's/DB_HOST=localhost/DB_HOST=localhost/g' /var/www/laravel/.env
sudo sed -i 's/DB_PORT=3306/DB_PORT=3306/g' .env
sudo sed -i 's/DB_DATABASE=Altschool/DB_DATABASE=Altschool/g' /var/www/laravel/.env
sudo sed -i 's/DB_USERNAME=Daniel/DB_USERNAME=Daniel/g' /var/www/laravel/.env
sudo sed -i 's/DB_PASSWORD=Sapphire/DB_PASSWORD=Sapphire/g' /var/www/laravel/.env
sudo php artisan key:generate
sudo php artisan storage:link
sudo php artisan migrate
sudo php artisan db:seed
sudo systemctl restart apache2
sudo php artisan serve
<img width="1127" alt="web app" src="https://github.com/da-nkz/laravel-assignment/assets/57856946/5c5b3209-e444-47af-a061-e170fddb1aec">
