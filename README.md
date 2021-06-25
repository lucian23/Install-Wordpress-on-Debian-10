# Install-Wordpress-on-Debian-10

apt install apache2 php7.3 libapache2-mod-php7.3 php7.3-common php7.3-mbstring php7.3-xmlrpc php7.3-soap php7.3-gd php7.3-xml php7.3-intl php7.3-mysql php7.3-cli php7.3-ldap php7.3-zip php7.3-curl

apt install default-mysql-server
mysql_secure_installation
Say “y” to all the questions

After that, it is necessary to create a new database for WordPress. Also, create a new user too.

mysql -u root -p
> CREATE DATABASE namedb;
> GRANT ALL PRIVILEGES on namedb.* TO 'namedb_user'@'localhost' IDENTIFIED BY 'namedb_passw';
> FLUSH PRIVILEGES;
> EXIT;

cd /tmp/
wget -c https://wordpress.org/latest.tar.gz
tar -xvzf latest.tar.gz
rm latest.tar.gz
mv wordpress/ /var/www/html/
cd /var/www/html/wordpress/
cp -rf . ..
cd ..
rm -R wordpress

cd /var/www/html/
cp wp-config-sample.php wp-config.php
sed -e 's/database_name_here/namedb/g' wp-config.php
sed -e 's/username_here/namedb_user/g' wp-config.php
sed -e 's/password_here/namedb_passw/g' wp-config.php
mkdir -p wp-content/uploads
chmod 755 wp-content/uploads

cd /etc/apache2/sites-available/
nano 000-default.conf

<VirtualHost *:80>
        ServerName name.com
        ServerAdmin your_email@gmail.com
        DocumentRoot /var/www/html/    
</VirtualHost>

For free ssl certbot:
apt install certbot python-certbot-apache -y
certbot --apache

apt update && apt upgrade
