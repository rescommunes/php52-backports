# Building PHP52 for Modern Ubuntu (2014)

## Install Dependencies

In addition to various dev tools we need v2.13 of autoconf 
 

```
apt-get -y install subversion
apt-get -y install build-essential autoconf2.13 
apt-get -y install apache2 apache2-dev apache2-threaded-dev apache2-mpm-prefork apache2-prefork-dev 
apt-get -y install libxml2-dev libxslt-dev
apt-get -y install openssl libssl-dev libcurl4-openssl-dev
apt-get -y install libbz2-dev
apt-get install   libssl-dev libbz2-dev libcurl3-dev libmcrypt-dev libmhash-dev freetds-dev libz-dev ncurses-dev libpcre3-dev unixODBC-dev  libreadline6-dev librecode-dev libtidy-dev libt1-dev

```

## Update some freetds paths
```
ln -s /usr/lib/x86_64-linux-gnu /usr/lib64 
ln -s /usr/lib/x86_64-linux-gnu/libsybdb.so /usr/lib/x86_64-linux-gnu/libsybdb.so.5 

```

## Download It

```
mkdir ~/src
cd ~/src

git clone https://github.com/rescommunes/php52-backports.git
cd php52-backports
```

## Build It

```
ldconfig

./buildconf --force


./configure --with-libdir=lib64 \
  --with-config-file-path=/etc/php5/apache2 \
  --with-pear=/usr/share/php \
  --enable-mbstring \
  --enable-bcmath \
  --enable-sockets \
  --enable-xml \
  --with-xsl \
  --with-bz2 \
  --with-curl \
  --with-gd \
  --with-unixODBC=/usr \
  --with-mssql=/usr \
  --with-openssl \
  --with-libxml-dir=/usr \
  --with-pcre-regex \
  --with-zlib \
  --with-pic  \
  --with-gettext \
  --with-iconv \
  --with-apxs2=/usr/bin/apxs2 \
  --disable-debug \
  --disable-rpath \
  --disable-dba \
  --disable-pdo \
  --disable-xmlreader \
  --disable-xmlwriter \
  --disable-dba \
  --without-mime-magic \
  --without-sqlite \
  --without-pspell \
  --without-mysql \
  --without-gd \
  --without-gdbm
  

make 

make install

libtool --finish /root/src/php52-backports/libs

```

## Place the extensions

```
mkdir /usr/local/lib/php/extensions

ln -s /root/src/php52-backports/libs `php-config --extension-dir`

```

## Add to Apache

```
a2enmod php5
service apache2 restart
```

## Test it

```
echo -e "<?php\n\tphpinfo();\n?>" >  /var/www/index.php

```
