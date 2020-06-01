# Wordpress Kurulum Adımları

## Apache ve PHP'yi kuralım

1. Uç birim veya terminal (konsole) açtıktan sonra aşağıdaki komutu çalıştıralım

```
sudo apt install apache2 php7.3 libapache2-mod-php7.3 php7.3-common php7.3-mbstring php7.3-xmlrpc php7.3-soap php7.3-gd php7.3-xml php7.3-intl php7.3-mysql php7.3-cli php7.3-ldap php7.3-zip php7.3-curl

```

2. Test amacıyla bir PHP kaynak kodu oluşturalım

`sudo nano /var/www/html/test.php`

```
<?php
phpinfo();
?>
```

PHP kodunu test etmek için tarayıcımızı açalım ve http://ipadresi/test.php sayfasını ziyaret edelim.

PHP ile ilgili ve sunucumuzla ilgili bir bilgi çıktığını gördüğümüzde bu sunucunun ve PHP'nin çalıştığı anlamına gelecektir.

## MySQL Kurulumu (MariaDB de olabilir)

Yine uç birimde şu komutu çalıştıralım

```
sudo apt install default-mysql-server default-mysql-client
```

## MySQL (MariaDB) Veritabanının ilk kurulumu yapılmalıdır sonrasında

```
sudo mysql_secure_installation
```

İlk olarak karşımıza çıkacak cümle şu olacak :

Root parolanızı giriniz :

Burada kullanıcı hesabınızın parolasını girmeniz gerekiyor.

Root parolasını değiştirmek ister misiniz ikinci sorudur.
Burada Hayır(H)/No(N) seçip ilerleyebilirsiniz.

Ancak sonrasında kesinlikle tüm gereksiz izinleri kaldırıp veritabanının güvenliğini ayarlamalısınız. Bunun için dört seçeneğin hepsine Evet(E)/Yes(Y) dememiz gerekiyor.

## MySQL erişimi

`sudo mysql -u root -p`
`CREATE DATABASE wordpress;`
`CREATE USER 'pardus'@'localhost' IDENTIFIED BY 'deneme';`
`GRANT ALL PRIVILEGES on wordpress.* TO 'pardus'@'localhost' IDENTIFIED BY 'deneme';`
`FLUSH PRIVILEGES;`
`EXIT;`

## Wordpress'i indiriyoruz

`cd /tmp/` dizinine geçiyoruz
`wget -c https://wordpress.org/latest.tar.gz` indiriyoruz.

Eğer wget yüklü değilse :

`sudo apt install wget` ile kurabiliriz

İndirdiğimiz Wordpress'i /var/www/html/ içine taşıyacağız ancak önce açacağız.

`tar -xvzf latest.tar.gz`
`sudo mv wordpress/ /var/www/html/`

Sonra kullanıcı izinlerini ve sahipliğini düzenliyoruz

`sudo chown -R www-data:www-data /var/www/html/wordpress/`
`sudo chmod 755 -R /var/www/html/wordpress/`

Pardus üzerinde varsayılan olarak nano editörü var nano ile Wordpress.conf dosyamızı düzenleyeceğiz

`sudo nano /etc/apache2/sites-available/wordpress.conf`

Aşağıdaki içeriği olduğu gibi kopyalayalım ve dosyanın içine yapıştıralım :

```
<VirtualHost *:80>
     ServerAdmin admin@your_domain.com
      DocumentRoot /var/www/html/wordpress
     ServerName your-domain.com

     <Directory /var/www/html/wordpress>
          Options FollowSymlinks
          AllowOverride All
          Require all granted
     </Directory>

     ErrorLog ${APACHE_LOG_DIR}/your-domain.com_error.log
     CustomLog ${APACHE_LOG_DIR}/your-domain.com_access.log combined

</VirtualHost>
```

Burada your-domain yerine kendi domain ismimizi yazacağız.

Sonrasında sırasıyla aşağıdaki komutları çalıştıralım :

`sudo ln -s /etc/apache2/sites-available/wordpress.conf /etc/apache2/sites-enabled/wordpress.conf`
`sudo a2enmod rewrite`
`sudo systemctl restart apache2` ile Apache servisini yeniden başlatmış olacağız.

Bu aşamada Apache'yi çalışır durumda ve Wordpress'i ilgili dizinin içinde yetkisi ve sahipliği düzenlenmiş olarak bulduğumuza göre yine tarayıcımızdan erişim sağlayarak kurulumu devam ettiriyoruz.

http://ipadresi/wordpress/ yazarak arayüze erişiyoruz.

Kurulum dilini seçiyoruz, kullanıcı hesabını ayarlıyoruz ve kurulumu tamamlıyoruz.
