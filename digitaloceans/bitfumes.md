# Adım Adım Bitfumes'in Kurulumu

### SSH Bağlantısı

```sh
# sunucuya ssh bağlantısı kurma
ssh root@_IP_ADRESI_ # şifreyi girip online ol
```

## Güvenli SSH Bağlantısı
Bilgisayarınızın local'inde şunları yapın

```sh
# ssh anahtarı üret
ssh-keygen -t rsa -b 4096 -C "server"
# id_server ve id_server.pub dosyalarını ürettik...

# ssh dizinindeki dosyaları listeleyerek oluşturduğunuz dosyayı görün
cd ~/.ssh/ & ls -la

# oluşturduğun id_server.pub dosyasının içeriğini kopyala
cat ~/.ssh/id_server.pub | pbcopy

# şu dosyanın içerisine id_server.pub içeriğini yapıştır
vim ~/.ssh/authorized_keys

```

Sunucuda root olarak:
```sh
# şu dosyanın içerisine id_server.pub içeriğini kaydet
vim ~/.ssh/authorized_keys
exit
```

Bilgisayarında:
```sh
# ssh ile sunucuya bağlanıyoruz
ssh -i ~/.ssh/id_server root@_ip_adresi_
```

Sunucuda:
```sh
# yeni bir kullanıcı açıyoruz
adduser tutkun

# şifre gibi bilgileri girdikten sonra
sudo su tutkun # ile kullanıcıyı değiştiriyoruz
exit # kullanıcıdan çıkış yapıyoruz

# usermod tanımlıyoruz
usermod -aG admin tutkun

# tekrar tutkun kullanıcısına giriyoruz
sudo su tutkun

mkdir ~/.ssh
vim ~/.ssh/authorized_keys
```

Bilgisayarda:
```sh
# dosyayı kopyalıyoruz
cat ~/.ssh/id_server.pub | pbcopy
```

Sunucuda:
```sh
vim ~/.ssh/authorized_keys # içerisine kopyalıyoruz
```

```sh
# artık tutkun ile giriş yapabiliriz
ssh -i ~/.ssh/id_server tutkun@_ip_adresi_

# root olarak tutkun'u tanımlamak için `PermitRootLogin yes` diğeri de `PasswordAuthentication yes` olan kısımları `no` yaparak dosyayı kaydedip çıkıyoruz.
sudo vim /etc/ssh/sshd_config
exit
```

Bilgisayarda:
```sh
# tekrar root girişi yapalım
ssh root@_ip_adresi_

# ssh servisini yeniden başlatalım
sudo service ssh restart
```


Bilgisayarda:
```sh
# tekrar root girişini deneyelim
ssh root@_ip_adresi_
```

O da ne?! Hata verdi:
```sh
Permission denied (publickey).
```

Bilgisayarda:
```sh
# bir de şunu deneyelim
ssh tutkun@_ip_adresi_
```

Tekrar hata aldık:

```sh
Permission denied (publickey).
```

Bilgisayarda bir de şunu deneyelim:
```sh
ssh -i ~/.ssh/id_server tutkun@_ip_adresi_
# aaa, girdi... :)
```

Bilgisayarda bir de `root` ile deneyelim:
```sh
ssh -i ~/.ssh/id_server root@_ip_adresi_
# aaa, girmedi :)
```

Artık `root` kullanıcısını güvene aldık. Bundan sonraki adımlarda `PHP`, `Nginx`, `MySQL`, `git`, `composer` ve diğer kurulumlarını yapacağız.

## PHP Kurulumu

Laravel gibi uygulamaların php ile rahatça çalışması için bazı eklentiler gereklidir.

Run following commands to install php >=7.1.3, use SUDO if you are not running commands as root user.

[Orjinal Anlatım](https://danyal.dk/blog/2018/03/07/install-laravel-required-php-extentions/)
[Bitfumes Anlatımı](https://bitfumes.com/courses/laravel/deploy-laravel-on-digital-ocean/tutorial-3)

**Ubuntu'yu Güncelle:**

```sh
sudo apt update && sudo apt upgrade
```

**Add the PHP repository from Ondřej:**

```sh
sudo apt install python-software-properties
sudo add-apt-repository ppa:ondrej/php
sudo apt update
sudo apt install php7.2
```

**PHP eklentiler kurulabilir:**
```sh
sudo apt install -y php7.2-cli \
                    php7.1-mcrypt \
                    php7.2-fpm \
                    php-pear \
                    php7.2-curl \
                    php7.2-dev \
                    php7.2-gd \
                    php7.2-mbstring \
                    php7.2-zip \
                    php7.2-mysql \
                    php7.2-xml \
                    php-token-stream \
                    php7.2-json

# versiyona bakalım:
php -v
```

Artık sunucuda gereken php ve eklentiler hazır olacak.

## MySQL Server Kurulumu

Sunucuda:
```sh
# otomatik kurulum için sudo apt install -y mysql-server denenebilir
sudo apt install mysql-server

# kurulumdan sonra
mysql -u root -p

# sonra da
mysql > SHOW DATABASES;
```

Sunucuda
```sh
# zip ve unzip kurulumunu yapıyoruz
sudo apt install zip \
                 unzip

# sunucuda tutkun kullanıcısında iken
ssh-keygen -t rsa -b 4096 -C "gitlab"
# enter ile isim kısmını (id_rsa) geçiyoruz
```
komutu ile sunucuya bir ssh anahtarı tanımlıyoruz.

Sunucuda:
```sh
# id_rsa.pub dosya içeriğini gitlab'a aktaracağız
cat ~/.ssh/id_rsa.pub
```

**NOT**: Yukarıdaki `id_rsa.pub`, `gitlab`'ın sunucuya bağlantısı için gereklidir.

Bilgisayarda:
```sh
git init
git add .
git commit -m "initial commit.."
git remote add origin git@gitlab.com:tutkun/proje_adi.git
git push -u origin master
```
kodlarını yazarak oluşturulan `proje_adi`'nı sunucuya yüklüyoruz

Şimdi `gitlab` üzerindeki projemize `sunucu` üzerindeki `id_rsa.pub` dosyasından aldığımız `ssh anahtarını` ekliyoruz.
Yazma yetkisi olmayacak. Sadece okuma yetkisi olacak.

Sunucuda:
```sh
# sunucudan gitlab'a bağlantı isteği yapıyoruz
ssh -T git@gitlab.com
```
komutu ile durumu kontrol ediyoruz. Bağlantı başarılı mesajı almamız gerekir.

Sunucuda:
```sh
cd ~/
git clone git@gitlab.com:tutkun/proje_adi.git html
```
ile `gitlab` projesini `sunucuya` kopyalıyoruz

## Composer Kurulumu

Şimdi `composer`ı kurmak için şu komutları sunucuya gönderiyoruz:
```sh
curl -sS https://getComposer.org/installer | sudo php -- --install-dir=/usr/local/bin --filename=composer
```

## Composer ile Laravel'i Ayarlama

Sunucuda:
```sh
# laravel dosyalarının bulunduğu dizinde composer.json dosyasının olduğu yerde
composer install --no-dev
```
komutu ile bağımlılıkları kuruyoruz. Bu kısımda `--no-dev` kısmı önemlidir. Yoksa aşağıdaki hataları alırsınız.

Sunucudaki hata:
```sh
Do not run Composer as root/super user! See https://getcomposer.org/root for details
Loading composer repositories with package information
Installing dependencies (including require-dev) from lock file
Your requirements could not be resolved to an installable set of packages.

  Problem 1
    - Installation request for erusev/parsedown 1.7.1 -> satisfiable by erusev/parsedown[1.7.1].
    - erusev/parsedown 1.7.1 requires ext-mbstring * -> the requested PHP extension mbstring is missing from your system.
  Problem 2
    - Installation request for laravel/framework v5.7.27 -> satisfiable by laravel/framework[v5.7.27].
    - laravel/framework v5.7.27 requires ext-mbstring * -> the requested PHP extension mbstring is missing from your system.
  Problem 3
  ...
```

Sunucuda:
```sh
# laravelin dizinine geçiyoruz
cd ~/html

# .env dosyasını oluşturuyoruz
cp .env.example .env

# artisan ile key:generate şifresi tanımlanıyor
php artisan key:generate
```

## Nginx Kurulumu

Sunucuda:
```sh
cd /etc/nginx/sites-avilable

# default dosyasını düzenle:
sudo vim default
```

Düzenlenen `default` dosyası:
```
root /var/www/html/public;
index index.html index.htm index.php
location / {
    try_files $uri $uri/ /index.php$is_args$args;
}

location ~ \.php$ {
    include snippets/fastcgi-php.conf;
    fastcgi_pass unix:/run/php/php7.2-fpm.sock;
}
```

Sunucuda:
```sh
# nginx'i yeniden başlatıyoruz
sudo service nginx restart
sudo service nginx reload
```