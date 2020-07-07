### İçerik:
1. [Sunucuda Yeni Kullanıcı Oturumu](/digitaloceans/new-user.md)
2. Digital Ocean Sunucu Yapılandırması
3. [SSH Bağlantısı](/digitaloceans/ssh-connection.md)
4. [Sunucunun Temel Ayarları](/digitaloceans/sunucu-temel-ayarlari.md)

## Digital Ocean İçin Sunucu Kurulum Ayarları

- Bu yazıda, sunucu yapılandırması adım adım anlatılacaktır.
- İlgili adımlar Envoyer ile otomatize edilecektir.

## Giriş
Bir "LAMP" yığını, bir sunucunun dinamik web sitelerini ve web uygulamalarını barındırmasını sağlamak için tipik olarak bir araya getirilen bir açık kaynaklı yazılım grubudur. 
Bu terim aslında Apache web sunucusuyla Linux işletim sistemini temsil eden bir kısaltmadır. 
Site verileri bir MySQL veritabanında saklanır ve dinamik içerik PHP tarafından işlenir. 
Bu [kılavuzda](https://www.digitalocean.com/community/tutorials/initial-server-setup-with-ubuntu-18-04), bir Ubuntu 18.04 sunucusuna bir LAMP yığını kuracağız.

## Ön şartlar:
Bu öğreticiyi tamamlamak için, root olmayan bir kullanıcı hesabına ve temel bir güvenlik duvarına sahip olan bir `Ubuntu 18.04` sunucusuna ihtiyacınız olacak.

Bu, `Ubuntu 18.04` için ilk sunucu kurulum kılavuzumuzu kullanarak yapılandırılabilir.

## Adım 1 - Apache'nin Kurulumu ve Güvenlik Duvarının Güncellenmesi 
Apache web sunucusu, dünyadaki en popüler web sunucuları arasındadır.

İyi bir şekilde belgelenmiştir ve bir çok web sitesi barındırmak için mükemmel bir seçim kılan web tarihi için geniş bir şekilde kullanılmaktadır.

### Ubuntu'nun (v18.04) Paket Yöneticisini (apt) Kullanarak `Apache` Kurmak:


Paket yöneticisini güncelle:
```sh
sudo apt update
```

`apache2` paketini kur:
```sh
sudo apt install apache2
```

Bu bir sudo komutu olduğundan, bu işlemler `root` ayrıcalıklarıyla yürütülür. Amaçlarınızı doğrulamak için sizden düzenli bir kullanıcı şifresi isteyecektir.

Parolanızı girdikten sonra, apt size hangi paketleri kurmayı planladığını ve ne kadar ekstra disk alanı kaplayacağını söyleyecektir. 
Devam etmek için `Y`'ye basın ve `ENTER`'a basın, kurulum devam edecektir.

## Web Trafiğine İzin Vermek için Güvenlik Duvarını Ayarlayın

Ardından, ilk sunucu kurulum talimatlarını izlediğinizi ve `UFW` güvenlik duvarını etkinleştirdiğinizi varsayarak, güvenlik duvarınızın `HTTP` ve `HTTPS` trafiğine izin verdiğinden emin olun. 
`UFW`'nin `Apache` için bir uygulama profili olup olmadığını kontrol edebilirsiniz:

```sh
sudo ufw app list
```

```sh
# Çıktı
Available applications:
  Apache
  Apache Full
  Apache Secure
  OpenSSH
```

`Apache Full` profiline bakarsanız, `80` ve `443` numaralı bağlantı noktalarına giden trafiği etkinleştirdiğini göstermelidir:

```sh
sudo ufw app info "Apache Full"
```

```sh
# Çıktı
Profile: Apache Full
Title: Web Server (HTTP,HTTPS)
Description: Apache v2 is the next generation of the omnipresent Apache web
server.

Ports:
  80,443/tcp
```

Bu profil için gelen HTTP ve HTTPS trafiğine izin verelim:

```sh
sudo ufw allow in "Apache Full"
```

Web tarayıcınızda sunucunuzun genel IP adresini ziyaret ederek her şeyin planlandığı gibi olduğunu doğrulamak için hemen bir anında kontrol yapabilirsiniz (bu bilgiye sahip değilseniz, genel IP adresinizin ne olduğunu öğrenmek için bir sonraki başlığın altındaki notu inceleyin). 
zaten):
```sh
http://your_server_ip
```

Bilgilendirme ve test amaçlı varsayılan Ubuntu 18.04 Apache web sayfasını göreceksiniz. 
Bunun gibi bir şeye benzemeli:

![Apachenin Kurulduğuna Dair Ekran Görüntüsü](https://assets.digitalocean.com/articles/how-to-install-lamp-ubuntu-18/small_apache_default_1804.png)

Bu sayfayı görürseniz web sunucunuz şimdi doğru bir şekilde yüklenmiştir ve güvenlik duvarınız üzerinden erişilebilir durumdadır.

## Sunucunuzun Genel IP Adresini Nasıl Bulunur?
Sunucunuzun genel IP adresinin ne olduğunu bilmiyorsanız, onu bulmanın birkaç yolu vardır.

Genellikle, bu sunucunuza SSH ile bağlanmak için kullandığınız adrestir.

Bunu komut satırından yapmanın birkaç farklı yolu vardır. Öncelikle `iproute2` araçlarını kullanarak IP adresinizi almak için aşağıdakini kullanabilirsiniz:

```sh
ip addr show eth0 | grep inet | awk '{ print $2; }' | sed 's/\/.*$//'
```

Bu size iki ya da üç satır geri verecek. Hepsi doğru adreslerdir, ancak bilgisayarınız yalnızca bunlardan birini kullanabilir, bu yüzden her birini denemekten çekinmeyin. Alternatif bir yöntem, sunucunuzu nasıl gördüğünü size bildirmek için dış tarafla iletişim kurmak için `curl` yardımcı programını kullanmaktır.

Bu, belirli bir sunucuya IP adresinizin ne olduğunu sorarak yapılır:

```sh
sudo apt install curl
curl http://icanhazip.com
```

IP adresinizi almak için kullandığınız yöntemden bağımsız olarak, varsayılan Apache sayfasını görüntülemek için web tarayıcınızın adres çubuğuna yazın.

## Adım 2 - MySQL'in Kurulumu 

Artık web sunucunuzu çalıştırıp çalıştırabildiğinize göre, MySQL'i kurmanın zamanı geldi. MySQL bir veritabanı yönetim sistemidir. Temel olarak, sitenizin bilgi depolayabileceği veritabanlarına düzenleme ve erişim sağlayacaktır.

Yine, bu yazılımı edinmek ve yüklemek için apt kullanın:

```sh
sudo apt install mysql-server
```

**Not:** Bu durumda, komuttan önce `sudo apt update` komutunu çalıştırmanız gerekmez. Bunun nedeni, yakın zamanda Apache'yi kurmak için yukarıdaki komutlarda uyguladığınızdır. Bilgisayarınızdaki paket dizini zaten güncel olmalıdır.

Bu komut da, yüklenecek paketlerin bir listesini ve alacağı disk alanını gösterir. Devam etmek için `Y` girin. 

Yükleme tamamlandığında, bazı tehlikeli varsayılanları kaldıracak ve veritabanı sisteminize erişimi kilitleyecek MySQL ile önceden yüklenmiş olarak gelen basit bir güvenlik komut dosyasını çalıştırın. 

Etkileşimli betiği çalıştırarak başlatın:

```sh
sudo mysql_secure_installation
```

Bu `VALIDATE PASSWORD PLUGIN`'i yapılandırmak isteyip istemediğinizi soracaktır.

**Not:** Bu özelliği etkinleştirmek bir yargılama çağrısıdır. 
Etkinleştirilirse, belirtilen kriterlere uymayan şifreler MySQL tarafından bir hata ile reddedilir. 
Bu, phpMyAdmin için Ubuntu paketleri gibi MySQL kullanıcı kimlik bilgilerini otomatik olarak yapılandıran yazılımla birlikte zayıf bir parola kullanırsanız sorunlara neden olur. 
Doğrulamayı devre dışı bırakmak güvenlidir, ancak veritabanı kimlik bilgileri için her zaman güçlü ve benzersiz şifreler kullanmalısınız.

`Y` cevabını evet veya cevap vermeden devam edecek başka bir şey için cevaplayın.

```sh
VALIDATE PASSWORD PLUGIN can be used to test passwords
and improve security. It checks the strength of password
and allows the users to set only those passwords which are
secure enough. Would you like to setup VALIDATE PASSWORD plugin?

Press y|Y for Yes, any other key for No:
```

`“Yes”` cevabını verirseniz, sizden bir şifre doğrulama seviyesi seçmeniz istenir. En güçlü seviye için `2` girerseniz, sayı içermeyen, büyük ve küçük harfler ve özel karakterler içeren veya genel sözlük kelimelerine dayanan bir şifre belirlemeye çalıştığınızda hatalar alacağınızı unutmayın.

```sh
There are three levels of password validation policy:

LOW    Length >= 8
MEDIUM Length >= 8, numeric, mixed case, and special characters
STRONG Length >= 8, numeric, mixed case, special characters and dictionary                  file

Please enter 0 = LOW, 1 = MEDIUM and 2 = STRONG: 1
```

`VALIDATE PASSWORD PLUGIN`'i ayarlamayı seçmiş olmanıza bakılmaksızın, sunucunuz daha sonra MySQL **root** kullanıcısı için bir şifre seçmenizi ve onaylamanızı ister. 
Bu, MySQL'de ayrıcalıkları arttıran bir idari hesaptır. 
Sunucunun kendisinin kök hesabına benzer olduğunu düşünün (şu anda yapılandırmakta olduğunuz hesabın MySQL'e özgü bir hesap olmasına rağmen). 
Bunun güçlü ve benzersiz bir şifre olduğundan emin olun ve boş bırakmayın.

Parola doğrulamasını etkinleştirdiyseniz, yeni girdiğiniz root parola için parola gücü gösterilir ve sunucunuz bu parolayı değiştirmek isteyip istemediğinizi soracaktır. Mevcut şifrenizden memnunsanız, istemde `"hayır"` için `N` girin:

```sh
Using existing password for root.

Estimated strength of the password: 100
Change the password for root ? ((Press y|Y for Yes, any other key for No) : n
```

Diğer sorular için `Y` tuşuna basın ve her komut satırında `ENTER` tuşuna basın. Bu, bazı anonim kullanıcıları ve test veritabanını kaldırır, uzak kök girişlerini devre dışı bırakır ve MySQL'in yaptığınız değişikliklere hemen saygı göstermesi için bu yeni kuralları yükler.

MySQL 5.7 (ve sonraki sürümleri) çalıştıran Ubuntu sistemlerinde, kök MySQL kullanıcısının bir parola yerine varsayılan olarak `auth_socket` eklentisini kullanarak kimlik doğrulaması yapacağını unutmayın. 
Bu, birçok durumda daha fazla güvenlik ve kullanılabilirlik sağlar, ancak harici bir programın (örneğin, phpMyAdmin) kullanıcıya erişmesine izin vermeniz gerektiğinde işleri karmaşık hale getirebilir.

MySQL'e **root** olarak bağlanırken bir şifre kullanmayı tercih ederseniz, kimlik doğrulama yöntemini `auth_socket` konumundan `mysql_native_password` konumuna getirmeniz gerekir. 

Bunu yapmak için, terminalinizden MySQL komut istemini açın:

```sh
sudo mysql
```

Ardından, her MySQL kullanıcı hesabınızın hangi kimlik doğrulama yöntemini aşağıdaki komutla kullandığını kontrol edin:

```sql
SELECT user,authentication_string,plugin,host FROM mysql.user;
```

```sh
# Çıktı
Output
+------------------+-------------------------------------------+-----------------------+-----------+
| user             | authentication_string                               | plugin                | host      |
+------------------+-----------------------------------------------------+-----------------------+-----------+
| root             |                                                     | auth_socket           | localhost |
| mysql.session    | *THIS_IS_NOT_A_VALID_PASSWORD_THAT_CAN_BE_USED_HERE | mysql_native_password | localhost |
| mysql.sys        | *THIS_IS_NOT_A_VALID_PASSWORD_THAT_CAN_BE_USED_HERE | mysql_native_password | localhost |
| debian-sys-maint | *CC744277A401A7D25BE1CA89AFF17BF607F876FF           | mysql_native_password | localhost |
+------------------+-----------------------------------------------------+-----------------------+-----------+
4 rows in set (0.00 sec)
```

Bu örnekte, `root` kullanıcının aslında `auth_socket` eklentisini kullanarak kimlik doğrulaması yaptığını görebilirsiniz. 
Root hesabını bir parola ile doğrulamak üzere yapılandırmak için aşağıdaki `ALTER USER` komutunu çalıştırın. 

Şifreyi değiştirmeyi seçtiğinizde güçlü bir şifre ile değiştirdiğinizden emin olun:

```sql
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password';
```

Ardından, sunucuya hibe tablolarını yeniden yüklemesini ve yeni değişikliklerinizi yürürlüğe koymasını söyleyen `FLUSH PRIVILEGES` uygulamasını çalıştırın:

```sql
FLUSH PRIVILEGES;
```

Root artık `auth_socket` eklentisini kullanarak kimliğini doğrulamadığını doğrulamak için kullanıcılarınızın her biri tarafından kullanılan kimlik doğrulama yöntemlerini tekrar kontrol edin:

```sql
SELECT user,authentication_string,plugin,host FROM mysql.user;
```

```sh
# Çıktı
Output
+------------------+-------------------------------------------+-----------------------+-----------+
| user             | authentication_string                     | plugin                | host      |
+------------------+-------------------------------------------+-----------------------+-----------+
| root             | *3636DACC8616D997782ADD0839F92C1571D6D78F | mysql_native_password | localhost |
| mysql.session    | *THISISNOTAVALIDPASSWORDTHATCANBEUSEDHERE | mysql_native_password | localhost |
| mysql.sys        | *THISISNOTAVALIDPASSWORDTHATCANBEUSEDHERE | mysql_native_password | localhost |
| debian-sys-maint | *CC744277A401A7D25BE1CA89AFF17BF607F876FF | mysql_native_password | localhost |
+------------------+-------------------------------------------+-----------------------+-----------+
4 rows in set (0.00 sec)
```

Bu örnek çıktıda, artık MySQL kullanıcısının bir parola kullanarak kimliğini doğruladığını görebilirsiniz. 
Bunu kendi sunucunuzda onayladıktan sonra, MySQL kabuğundan çıkabilirsiniz:

```sh
exit
```

Bu noktada, veritabanı sisteminiz kurulmuştur ve LAMP yığınının son bileşeni olan PHP'yi kurmaya devam edebilirsiniz.

## SequelPro Üzerinden SSH Bilgileri İle MySQL'e Bağlanma
Aşağıdaki örnek bilgileri de kullanarak veritabanı sunucusuna ssh bilgileriniz ile erişebilirsiniz.

```sh
# MySQL Bilgileri
MySQL Host: 127.0.0.1
Username: root
Password: şifreniz
Database: (opsiyonel)
Port: (opsiyonel)
# SSH Bilgileri
SSH Host: SUNUCU_IP_ADRESINIZ
SSH User: root
SSH Key: ~/.ssh/id_rsa.pub
SSH Port: 22
```

**Benim Notum:**
Eğer gerek duyulursa aşağıdaki komutlar ile firewall üzerinde hangi portun açılması gerektiğini de belirtebiliriz. Ancak şimdilik gerek yok!

```sh
sudo ufw allow 3306/tcp
sudo service ufw restart
```

## Adım 3 - PHP'yi Kurmak 
PHP, kurulumunuzun dinamik içeriği görüntülemek için kod işleyecek bileşenidir. Komut dosyalarını çalıştırabilir, bilgi almak için MySQL veritabanlarınıza bağlanabilir ve işlenen içeriği görüntülemek için web sunucunuza teslim edebilir.

PHP'yi kurmak için bir kez daha `apt` sisteminden yararlanın. 
Ek olarak, PHP kodunun Apache sunucusu altında çalışabilmesi ve MySQL veritabanınızla konuşabilmesi için bazı yardımcı paketler ekleyin:

```sh
sudo apt install php libapache2-mod-php php-mysql
```

Bu PHP'yi sorunsuzca kurmalıdır. 
Bunu bir dakika içinde test edeceğiz. 
Çoğu durumda, bir dizin istendiğinde Apache'nin dosya sunma şeklini değiştirmek isteyeceksiniz. 
Şu anda, bir kullanıcı sunucudan bir dizin isterse, Apache ilk önce `index.html` adlı bir dosyayı arayacaktır. 
Web sunucusuna PHP dosyalarını başkalarına tercih etmesini söylemek istiyoruz, bu yüzden Apache'yi bir `index.php` dosyasını önce arayın.

Bunu yapmak için, dir.conf dosyasını root haklarına sahip bir metin editöründe açmak için bu komutu yazın:

```sh
sudo nano /etc/apache2/mods-enabled/dir.conf
# or
sudo vim /etc/apache2/mods-enabled/dir.conf
# ot
sudo vi /etc/apache2/mods-enabled/dir.conf
```

Bunun gibi görünecek:

```sh
# /etc/apache2/mods-enabled/dir.conf
<IfModule mod_dir.c>
    DirectoryIndex index.html index.cgi index.pl index.php index.xhtml index.htm
</IfModule>
```

PHP dizin dosyasını (yukarıda vurgulanmış olarak) `DirectoryIndex` belirtiminden sonraki ilk konuma taşıyın, şöyle:

```sh
# /etc/apache2/mods-enabled/dir.conf
<IfModule mod_dir.c>
    DirectoryIndex index.php index.html index.cgi index.pl index.xhtml index.htm
</IfModule>
```

İşiniz bittiğinde, `CTRL + X` tuşlarına basarak dosyayı kaydedin ve kapatın. 
`Y` yazarak kaydetme işlemini onaylayın ve ardından dosya kaydetme konumunu doğrulamak için `ENTER`'a basın. 
Bundan sonra, değişikliklerin tanınması için Apache web sunucusunu yeniden başlatın.

Bunu yazarak yapın:

```sh
sudo systemctl restart apache2
```

`systemctl` kullanarak `apache2` servisinin durumunu da kontrol edebilirsiniz:

```sh
sudo systemctl status apache2
```

```sh
# Basit Çıktı
● apache2.service - LSB: Apache2 web server
   Loaded: loaded (/etc/init.d/apache2; bad; vendor preset: enabled)
  Drop-In: /lib/systemd/system/apache2.service.d
           └─apache2-systemd.conf
   Active: active (running) since Tue 2018-04-23 14:28:43 EDT; 45s ago
     Docs: man:systemd-sysv-generator(8)
  Process: 13581 ExecStop=/etc/init.d/apache2 stop (code=exited, status=0/SUCCESS)
  Process: 13605 ExecStart=/etc/init.d/apache2 start (code=exited, status=0/SUCCESS)
    Tasks: 6 (limit: 512)
   CGroup: /system.slice/apache2.service
           ├─13623 /usr/sbin/apache2 -k start
           ├─13626 /usr/sbin/apache2 -k start
           ├─13627 /usr/sbin/apache2 -k start
           ├─13628 /usr/sbin/apache2 -k start
           ├─13629 /usr/sbin/apache2 -k start
           └─13630 /usr/sbin/apache2 -k start
```

Bu durum çıktısından çıkmak için `Q` düğmesine basın. 

PHP'nin işlevselliğini arttırmak için bazı ek modüller kurma seçeneğiniz vardır. PHP modülleri ve kütüphaneleri için mevcut seçenekleri görmek için `apt search` komutunun sonuçlarını, diğer komutların çıktısını kaydırmanıza izin veren bir çağrı cihazı olarak `less` içine aktarın:

```sh
apt search php- | less
```

Yukarı ve aşağı kaydırmak için ok tuşlarını kullanın ve çıkmak için `Q` düğmesine basın. 

Sonuçların tümü, yükleyebileceğiniz isteğe bağlı bileşenlerdir. Size her biri için kısa bir açıklama verecek:

```sh
bandwidthd-pgsql/bionic 2.0.1+cvs20090917-10ubuntu1 amd64
  Tracks usage of TCP/IP and builds html files with graphs

bluefish/bionic 2.2.10-1 amd64
  advanced Gtk+ text editor for web and software development

cacti/bionic 1.1.38+ds1-1 all
  web interface for graphing of monitoring systems

ganglia-webfrontend/bionic 3.6.1-3 all
  cluster monitoring toolkit - web front-end

golang-github-unknwon-cae-dev/bionic 0.0~git20160715.0.c6aac99-4 all
  PHP-like Compression and Archive Extensions in Go

haserl/bionic 0.9.35-2 amd64
  CGI scripting program for embedded environments

kdevelop-php-docs/bionic 5.2.1-1ubuntu2 all
  transitional package for kdevelop-php

kdevelop-php-docs-l10n/bionic 5.2.1-1ubuntu2 all
  transitional package for kdevelop-php-l10n
…
:
```

Her modülün ne yaptığı hakkında daha fazla bilgi edinmek için, onlar hakkında daha fazla bilgi için internette arama yapabilirsiniz. 
Alternatif olarak, yazarak paketin uzun tanımına bakınız:

```sh
apt show package_name
```

Modülün sağladığı işlevsellik hakkında daha uzun bir açıklama yapacak olan `Description` adlı bir alanla birlikte çok fazla çıktı olacaktır.

Örneğin, `php-cli` modülünün ne yaptığını bulmak için şunu yazabilirsiniz:

```sh
apt show php-cli
```

Çok miktarda başka bilginin yanı sıra, şuna benzeyen bir şey bulacaksınız:

```sh
# Çıktı
…
Description: command-line interpreter for the PHP scripting language (default)
 This package provides the /usr/bin/php command interpreter, useful for
 testing PHP scripts from a shell or performing general shell scripting tasks.
 .
 PHP (recursive acronym for PHP: Hypertext Preprocessor) is a widely-used
 open source general-purpose scripting language that is especially suited
 for web development and can be embedded into HTML.
 .
 This package is a dependency package, which depends on Ubuntu's default
 PHP version (currently 7.2).
…
```

Araştırmadan sonra bir paket yüklemek istediğinize karar verirseniz, diğer yazılım için yaptığınız gibi `apt install` komutunu kullanarak bunu yapabilirsiniz. 

`php-cli`'nin ihtiyacınız olan bir şey olduğuna karar verdiyseniz, şunu yazabilirsiniz:

```sh
sudo apt install php-cli
```

Birden fazla modül kurmak istiyorsanız, bunu, `apt install` komutunu izleyerek, bir boşlukla ayırarak her birini listeleyerek, şöyle yapabilirsiniz:

```sh
sudo apt install package1 package2 ...
```

Bu noktada LAMP yığınınız kurulur ve yapılandırılır. 
Daha fazla değişiklik yapmadan veya bir uygulamayı dağıtmadan önce, ele alınması gereken herhangi bir sorun olması durumunda PHP yapılandırmanızı proaktif olarak test etmeniz faydalı olacaktır.

## Adım 4 - Web Sunucunuzun PHP'yi İşlediğini Test Etme

Sisteminizin PHP için doğru bir şekilde yapılandırıldığını test etmek için `info.php` adında çok basit bir PHP betiği oluşturun. 
Apache'nin bu dosyayı bulması ve doğru şekilde sunması için "web root" adı verilen çok özel bir dizine kaydedilmesi gerekir.

Ubuntu 18.04'te bu dizin `/var/www/html/` konumunda bulunur. Çalıştırarak dosyayı bu konumda oluşturun:

```sh
sudo nano /var/www/html/info.php
# or
sudo vim /var/www/html/info.php
```

Bu boş bir dosya açacaktır. Dosyanın içine geçerli bir PHP kodu olan aşağıdaki metni ekleyin:

```php
# /var/www/html/info.php
<?php
phpinfo();
?>
```

İşiniz bittiğinde dosyayı kaydedin ve kapatın. 

Artık web sunucunuzun bu PHP betiği tarafından oluşturulan içeriği doğru şekilde gösterip gösteremediğini test edebilirsiniz. 
Bunu denemek için, web tarayıcınızda bu sayfayı ziyaret edin. Sunucunuzun genel IP adresine tekrar ihtiyacınız olacak. 

Ziyaret etmek isteyeceğiniz adres:

```sh
http://your_server_ip/info.php
```

Geldiğiniz sayfa şöyle görünmeli:

![Php Info](https://assets.digitalocean.com/articles/how-to-install-lamp-ubuntu-18/small_php_info_1804.png)

Bu sayfa PHP'niz açısından sunucunuz hakkında temel bilgiler sağlar. Hata ayıklamak ve ayarlarınızın doğru uygulandığından emin olmak için kullanışlıdır. 

Bu sayfayı tarayıcınızda görebilirseniz, PHP'niz beklendiği gibi çalışıyor. Muhtemelen bu testten sonra bu dosyayı kaldırmak isteyeceksiniz çünkü sunucunuz hakkında yetkisiz kullanıcılara bilgi verebilir. 

Bunu yapmak için aşağıdaki komutu çalıştırın:

```sh
sudo rm /var/www/html/info.php
```

Bilgiye daha sonra tekrar erişmeniz gerekirse, bu sayfayı her zaman yeniden oluşturabilirsiniz.

## Ek Not:
Apache ayarları için `/etc/apache2/...conf` dosyalarını gözden geçir

## Sonuç

Artık yüklü bir LAMP yığına sahip olduğunuza göre, daha sonra yapılacaklar için birçok seçeneğiniz vardır. 
Temel olarak, sunucunuza çoğu türde web sitesi ve web yazılımı yüklemenizi sağlayacak bir platform yüklediniz. 
Hemen bir sonraki adım olarak, web sunucunuzla olan bağlantıların HTTPS aracılığıyla sağlanarak güvenli olduğundan emin olmalısınız. 
Buradaki en kolay seçenek, sitenizi ücretsiz bir TLS / SSL sertifikasıyla güvence altına almak için Let's Encrypt kullanmaktır. 

Diğer bazı popüler seçenekler:
 * İnternetteki en popüler içerik yönetim sistemini [Wordpress'i kurun](https://www.digitalocean.com/community/tutorials/how-to-install-wordpress-with-lamp-on-ubuntu-16-04). 
 * MySQL veritabanlarınızı web tarayıcısından yönetmenize yardımcı olacak [PHPMyAdmin](https://www.digitalocean.com/community/tutorials/how-to-install-and-secure-phpmyadmin-on-ubuntu-18-04)'i ayarlayın. 
 * Sunucunuza ve sunucunuzdan dosya aktarmak için [SFTP'yi nasıl kullanacağınızı](https://www.digitalocean.com/community/articles/how-to-use-sftp-to-securely-transfer-files-with-a-remote-server) öğrenin.

## Digital Oceans Çevirisi:
Bu makale şu [kaynağın](https://www.digitalocean.com/community/tutorials/how-to-install-linux-apache-mysql-php-lamp-stack-ubuntu-18-04) çevirisidir.
