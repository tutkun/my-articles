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

## Composer Kurulumu

Şimdi `composer`ı kurmak için şu komutları sunucuya gönderiyoruz:
```sh
curl -sS https://getComposer.org/installer | sudo php -- --install-dir=/usr/local/bin --filename=composer
```

Daha sonra:
```sh
composer install --no-dev
```
komutu ile bağımlılıkları kuruyoruz. Bu kısımda --no-dev kısmı önemlidir. Yoksa aşağıdaki hataları alırsınız.

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
