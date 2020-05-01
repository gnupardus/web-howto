##[LAMP](https://www.linuxbabe.com/debian/install-lamp-stack-debian-10-buster)

#### Sistemi güncelleyelim
     1. sudo apt-get update
     2. sudo apt-get upgrade
#### Apache Web Sunucusunu kuralımm
     sudo apt install apache2 apache2-utils
#### Apache çalışıyor mu diye kontrol edelim
     sudo systemctl status apache2
     enabled;
     active (running) görüyorsak çalışıyor demektir.
     Eğer çalışmıyorsa :
     sudo systemctl start apache2
     Apache'yi otomatik olarak boot/başlatma sırasında çalışmasını sağlamak için :
     sudo systemctl enable apache2
     Apache sürümünü test etmek için
     sudo apache2 -v
     Sonrasında ipadresinizi yazarak apache sayfasına erişebilirsiniz.
#### Apache için yetki verelim
     sudo chown www-data:www-data /var/www/html -R
#### MySQL yükleyelim
     sudo apt-get install default-mysql-server default-mysql-client
     MySQL çalışıyor mu diye kontrol edelim
     sudo systemctl status mysql
     enabled;
     active (running) görüyorsak çalışıyor demektir.
     çalışmıyorsa :
     sudo systemctl start mysql
     Boot sonrası otomatik başlaması için
     sudo systemctl enable mysql
##### MySQL kurulumu için
      sudo mysql_secure_installation
      Bu aşamada kullanıcı hesabınızın parolasını girelim
      Sonra yeni parola belirleyelim
      Sonrasında güvenlik sebebiyle tüm seçenekleri Evet / Yes olarak ayarlayalım
      sudo mysql -u root -p ile mysql arayüzüne erişebiliriz bu aşamada exit; diyerek çıkacağız
      mysql sürümünü kontrol için
      mysql --version komutunu çalıştıralım
####  PHP7.3 yükleyelim
      sudo apt install php7.3 libapache2-mod-php7.3 php7.3-mysql php-common php7.3-cli php7.3-common php7.3-json php7.3-opcache php7.3-readline
      İlgili modülü aktif edelim ve Apache servisini yeniden başlatalım :
      sudo a2enmod php7.3
      sudo systemctl restart apache2
##### PHP sürümünü kontrol için çalıştıracağımız komut :
      php --version
      PHP test için
      sudo nano /var/www/html/info.php ile dosyayı açalım ve içine
      <?php phpinfo(); ?> yazıp Ctrl-X ve Evet/Yes yazıp Enter'a basıp kaydedelim.
      ipadresi/info.php yazdığımızda karşımıza PHP arayüzü çıkacaktır.
##### Bazı durumlarda PHP-FPM kullanmak gerekebilir nasıl yapılacağı :
      Apache PHP7.3 modülünü devre dışı bırakmak için : 
      sudo a2dismod php7.3
      PHP-FPM yüklemek için
      sudo apt install php7.3-fpm
      proxy_fcgi ve setenvif modüllerini aktif edelim
      sudo a2enmod proxy_fcgi setenvif
      /etc/apache2/conf-available/php7.3-fpm.conf konfigürasyon dosyasını aktif edelim
      sudo a2enconf php7.3-fpm
      Apache servisini yeniden başlatalım
      sudo systemctl restart apache2
      Yine ipadresi/info.php ile PHP arayüzüne girebiliriz

