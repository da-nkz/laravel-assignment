# Laravel Deployment 
<br>To update the linux reposiroy, you run the `sudo apt update` command<br>
<br>This updates your linux system and checks for dependencies the system needs.<br>

![Linux system update](<sudo apt update.png>)


<br>We install the apache web server using the `sudo apt install apache2 -y`<br>
<br>We’ll be hosting out laravel application on this server<br>.

![apache homepage](<apache page.png>)


<br>Update linux repository again by running `sudo apt update`<br>


### Install php8.2

<br>We'll install php by using the `sudo apt install php8.2 -y` command.<br>
![php --version](<php home.png>)


### Restart your apache server
<br>We restart the apache server for changes to be made and for php to integrate with tht server. `sudo systemctl restart apache2`<br>

### Install mysql
<br>`sudo apt install mysql -y`<br>
<br>`sudo systemctl start mysql`<br>

### Change directory in the bin directory
<br>cd /usr/bin<br>

### Install composer
<br>`sudo curl -sS https://getcomposer.org/installer | sudo php -y`<br>

### Move the content of the default composer.phar
<br>`sudo mv composer.phar composer`<br>

### We'll change our working directory to /var/www directory so we can clone our laravel repo
<br>`cd /var/www/`<br>
<br>`sudo git clone https://github.com/da-nkz/laravel.git`<br>
<br>`sudo chown -R root laravel`<br>
<br>`cd laravel/`<br>

<br>We'll run the following commands to install composer autoloader ensure that any dependencies unique to the laravel program are locked in.<br>
<br>`sudo composer install --optimize-autoloader --no-dev`<br>
<br>`sudo composer update`<br>

<br>We'll copy the content of the default env file to .env to edit the MYSQL Database settings<br>
<br>sudo cp .env.example .env<br>

`sudo chown -R www-data storage`
`sudo chown -R www-data bootstrap/cache`

###We'll run the commands below to create new default laravel homepage 
<br>`cd /etc/apache2/sites-available/`<br>
<br>`sudo touch laravel.conf`<br>
<br>`sudo cp /home/vagrant/latest.conf laravelnew.conf`<br>
<br>`sudo a2ensite laravelnew.conf`<br>
<br>`sudo systemctl restart apache2`<br>


![laravel app](<web app.png>)