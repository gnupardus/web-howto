# [phpMyAdmin](https://www.linuxbabe.com/debian/install-phpmyadmin-apache-lamp-debian-10-buster)

## phpMyAdmin'i indirelim

`wget https://files.phpmyadmin.net/phpMyAdmin/4.9.0.1/phpMyAdmin-4.9.0.1-all-languages.zip`
Dosyayı çıkarmak için
`sudo apt install unzip` yükleyelim
`unzip phpMyAdmin-4.9.0.1-all-languages.zip` ile dosyayı çıkaralım
phpMyAdmin 4.9'u /usr/share/ dizinine taşıyalım bunun için sudo kullanmamız gerekiyor
`sudo mv phpMyAdmin-4.9.0.1-all-languages /usr/share/phpmyadmin`
sahipliği düzenleyelim
`sudo chown -R www-data:www-data /usr/share/phpmyadmin`

## phpMyAdmin için veritabanı ve kullanıcı oluşuralım

`sudo mysql -u root -p`
`CREATE DATABASE phpmyadmin DEFAULT CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;`
`CREATE USER 'pardus'@'localhost' IDENTIFIED BY 'deneme';`
`GRANT ALL ON phpmyadmin.* TO 'pardus'@'localhost' IDENTIFIED BY 'deneme';`
`FLUSH PRIVILEGES;`
`EXIT;`

## PHP Modüllerini yükleyelim

`sudo apt install php-imagick php-phpseclib php-php-gettext php7.3-common php7.3-mysql php7.3-gd php7.3-imap php7.3-json php7.3-curl php7.3-zip php7.3-xml php7.3-mbstring php7.3-bz2 php7.3-intl php7.3-gmp`
Apache'yi yeniden başlatalım
`sudo systemctl restart apache2`

## phpMyAdmin için Apache konfigürasyon dosyasını oluşturalım

`sudo nano /etc/apache2/conf-available/phpmyadmin.conf`

İçeriği kopyalayalım :

```

## phpMyAdmin default Apache configuration


Alias /phpmyadmin /usr/share/phpmyadmin

<Directory /usr/share/phpmyadmin>
Options SymLinksIfOwnerMatch
DirectoryIndex index.php

<IfModule mod_php5.c>
<IfModule mod_mime.c>
AddType application/x-httpd-php .php
</IfModule>
<FilesMatch ".+\.php$">
SetHandler application/x-httpd-php
</FilesMatch>

php_value include_path .
php_admin_value upload_tmp_dir /var/lib/phpmyadmin/tmp
php_admin_value open_basedir /usr/share/phpmyadmin/:/etc/phpmyadmin/:/var/lib/phpmyadmin/:/usr/share/php/php-gettext/:/usr/share/php/php-php-gettext/:/usr/share/javascript/:/usr/share/php/tcpdf/:/usr/share/doc/phpmyadmin/:/usr/share/php/phpseclib/
php_admin_value mbstring.func_overload 0
</IfModule>
<IfModule mod_php.c>
<IfModule mod_mime.c>
AddType application/x-httpd-php .php
</IfModule>
<FilesMatch ".+\.php\$">
SetHandler application/x-httpd-php
</FilesMatch>

php_value include_path .
php_admin_value upload_tmp_dir /var/lib/phpmyadmin/tmp
php_admin_value open_basedir /usr/share/phpmyadmin/:/etc/phpmyadmin/:/var/lib/phpmyadmin/:/usr/share/php/php-gettext/:/usr/share/php/php-php-gettext/:/usr/share/javascript/:/usr/share/php/tcpdf/:/usr/share/doc/phpmyadmin/:/usr/share/php/phpseclib/
php_admin_value mbstring.func_overload 0
</IfModule>

</Directory>

(Disallow web access to directories that don't need it)

<Directory /usr/share/phpmyadmin/templates>
Require all denied
</Directory>
<Directory /usr/share/phpmyadmin/libraries>
Require all denied
</Directory>
<Directory /usr/share/phpmyadmin/setup/lib>
Require all denied
</Directory>
```

Ctrl-X Evet / Yes ve Enter ile dosyadan çıkalım.

Konfigürasyonu aktif hale getirmek için :
`sudo a2enconf phpmyadmin.conf`
Ayrıca phpMyAdmin temp klasörü oluşturmamız gerekli
`sudo mkdir -p /var/lib/phpmyadmin/tmp`
`sudo chown www-data:www-data /var/lib/phpmyadmin/tmp`
Apache'yi yeniden başlatıyoruz bu işlemden sonra
`sudo systemctl reload apache2`

### Artık phpMyAdmin hazır

ipadresiniz/phpmyadmin ile arayüze erişebilirsiniz.
