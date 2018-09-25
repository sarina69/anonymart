Automatic Setup
===
*Currently available only for Debian & CentOS*
```bash
cd /tmp
wget https://raw.githubusercontent.com/annularis/shop/master/install.sh
svn checkout https://github.com/annularis/shop/trunk/installer
chmod a+x install.sh installer/*
./install.sh
```

Manual Setup
===
```bash
# Annularis setup
# tested in debian 9.2 in virtualbox

# be a root
sudo su

# dependency
apt-get install nginx mysql-server mysql-client php libapache2-mod-php php-mcrypt php-gnupg php-mysql php-gmp php-curl php-bcmath php-gd git mysqli mcrypt curl gmp gd unzip atool

# install annularis source (from git)
cd /var/www/html
git clone https://github.com/annularis/shop
chown www-data:www-data -Rv /shop
cd shop/install

# Enable Mod Rewrite
a2enmod rewrite
service apache2 restart

# install bitcoin
cd /tmp
wget https://bitcoin.org/bin/bitcoin-core-0.16.3/bitcoin-0.16.3-x86_64-linux-gnu.tar.gz # or 32bit
tar xvf bitcoin-0.16.3-x86_64-linux-gnu.tar.gz
cd bitcoin-0.16.3
cp bin/* /usr/local/bin/

# run bitcoin
bitcoind -daemon -testnet -rpcport="7530" -rpcuser="bitcoinuser" -rpcpassword="bitcoinpass"

# change AllowOverride All for /var/www
# vim /etc/apache2/apache2.conf

# change DocumentRoot to /var/www/shop
# vim /etc/apache2/sites-enabled/000-default.conf
# service apache2 restart

# add mysql user and add database for him
CREATE DATABASE annularis;
CREATE USER annularis IDENTIFIED BY 'password';
GRANT ALL PRIVILEGES ON annularis.* TO annularis@localhost IDENTIFIED BY 'password';

cp htaccess.sample .htaccess
# comment out 'RewriteBase /shop'
vim .htaccess

# access to /install/ and follow instruction there
http://localhost/install/

# And if `php /var/www/html/shop/index.php db` succeeds, everything's done!
```
