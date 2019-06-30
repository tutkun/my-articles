### İçerik:
1. [Digital Ocean Sunucu Yapılandırması](https://gitlab.com/tutkun/reading/blob/master/digitaloceans/readme.md)
2. [SSH Bağlantısı](https://gitlab.com/tutkun/reading/blob/master/digitaloceans/ssh-connection.md)
3. Sunucunun Temel Ayarları

# Bu Kısımda Sunucunun Temel Ayarlarını Yapacağız

* Composer Kurulumu
* NPM Kurulumu
* Supervisor Kurulumu
* Redis Kurulumu
* 

## PHP Eklentilerinin Kurulumu
Laravel gibi uygulamaların php ile rahatça çalışması için bazı eklentiler gereklidir.

Run following commands to install php >=7.1.3, use SUDO if you are not running commands as root user.

**Ubuntu'yu Güncelle:**
  sudo apt update && sudo apt upgrade
**Add the PHP repository from Ondřej:**
  apt install python-software-properties
  add-apt-repository ppa:ondrej/php
  apt update
  apt install php7.2
**PHP eklentiler kurulabilir:**
```sh
sudo apt install php-pear php7.2-curl php7.2-dev php7.2-gd php7.2-mbstring php7.2-zip php7.2-mysql php7.2-xml php-token-stream php7.2-json
```

## Composer Kurulumu

Şimdi `composer`ı kurmak için şu komutları sunucuya gönderiyoruz:
```sh
curl -sS https://getComposer.org/installer | sudo php -- --install-dir=/usr/local/bin --filename=composer
```

Daha sonra:
```sh
composer install --no-dev
```
komutu ile bağımlılıkları kuruyoruz. Bu kısımda `--no-dev` kısmı önemlidir. Yoksa aşağıdaki hataları alırsınız.

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

## NPM Kurulumu


## Supervisor Kurulumu

```sh
sudo apt install supervisor
```

## Redis Kurulumu
