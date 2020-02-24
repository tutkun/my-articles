# NGINX Kurulumu ve Kullanımı Hakkında Herşey

## Debian/Ubuntu Ortamlarında Kurulum

Bir Ubuntu ya da Debian tabanlı işletim sistemlerinde NGINX'in kurulumu...

`/etc/apt/sources.list.d/nginx.list` olarak adlandırılmış olan dosya şu aşağıdakileri içerir:

```sh
// CODENAME için detaylar aşağıda...
deb http://nginx.org/packages/mainline/OS/ CODENAME nginx
deb-src http://nginx.org/packages/mainline/OS/ CODENAME nginx
```

Yukarıdaki CODENAME ile belirtilen kısımlar, Ubuntu veya Debian sistemlerin dağıtım sürümüdür. Siz, kendi dağıtım sisteminizin adı ile onları değiştirmelisiniz.

Debian için; `jessie` veya `stretch`
Ubuntu için ise; `bionic`, `artful`, `xenial` veya `trusty`

Artık aşağıdaki komutları çalıştırabilirsiniz:
```sh
wget http://nginx.org/keys/nginx_signing.key
apt-key add nginx_signing.key
apt-get update
apt-get install -y nginx
/etc/init.d/nginx start
```

Yukarıda, `apt` paket yönetim sistemi, Official NGINX paket deposu birim dosyasını oluşturur. NGINX kurulumu için güncellemeleri yapar ve Nginx'i kurar. Ardından nginx servisi başlatılır.


## RedHat/CentOS da Kurulum

`/etc/yum.repos.d/nginx.repo` dosyasını oluşturup içine şunları ekleyin:
```sh
[nginx]
name=nginx repo
baseurl=http://nginx.org/packages/mainline/OS/OSRELEASE/$basearch/
gpgcheck=0
enabled=1
```

Yukarıdaki `OS` yazan yeri sisteminizin dağıtım sürümüne göre `rhel` ya da `centos` olarak değiştirin. `OSRELEASE` kısmını da `6.X` için `6` ya da `7.X` için `7` olarak değiştirin ve sonra aşağıdaki komutları çalıştırın.

```sh
yum -y install nginx
systemctl enable nginx
systemctl start nginx
# TCP için 80 portunu açan firewall komutu
firewall-cmd --permanent --zone=public --add-port=80/tcp
firewall-cmd --reload
```


## Kurulumu Doğrulama

Nginx'in başarılı bir şekilde kurulduğunu doğrulamak için Nginx versiyonunu kontrol edebilirsiniz.

```sh
nginx -v
# çıktı:
# nginx version: nginx/1.15.3
```

Şu şekilde nginx'in çalıştığını doğrulayabilirsiniz:

```sh
ps -ef | grep nginx
# çıktı:
# root 1738 1 0 19:54 ? 00:00:00 nginx: master process
# nginx 1739 1738 0 19:54 ? 00:00:00 nginx: worker process
```

`ps` komutu, çalışan tüm işlemlerin bir listesini verir. `grep`e girilen ikinici parametre ile bu listeyi filtreleyerek daraltabilirsiniz. Örn: `... | grep nginx`

Artık `curl` ile `localhost`a bir istek atarak Nginx'in varsayılan HTML sayfasını görebilirsiniz.

```sh
curl localhost
```

## Anahtar Dosyaları, Komutları ve Dizinleri

Nginx'in en önemli komutlarını ve dizinlerini anlamak isterseniz;

### NGINX Dosya ve Dizinleri:

*/etc/nginx/* :
Nginx sunucusu için varsayılan ayarların bulunduğu kök dizinidir. Bu dizinde Nginx'in nasıl yapılandırıldığını anlatan ayar dosyalarını bulacaksınız.

*/etc/nginx/nginx.conf* :
Nginx servisi tarafından giriş noktası olarak kullanılan varsayılan ayarların bulunduğu dosyadır. Bu ayar dosyası; diğer Nginx ayar dosyalarını, referansları, worker process, tuning, logging ve dinamik modülleri yüklemek gibi şeyler için global ayar dosyasıdır.

*/etc/nginx/conf.d/* :
Bu dizin, varsayılan HTTP sunucu ayarlarını içeren dosya dizinidir. Bu dizindeki `.conf` dosyalarını, `/etc/nginx/nginx.conf` dosyasından üst-seviye http block dosyaları olarak içe aktarmak sureti ile çağırır.

*/var/log/nginx/* :
Nginx için varsayılan Log dizinidir. Bu dizinde bir `access.log` ve `error.log` dosyası bulacaksınız. `access.log` dosyası, Nginx sunucularına gelen her bir isteği içerir. `error.log` dosyası da eğer `debug` modülü `enabled` ise debug bilgisi ve hata olaylarını içerir.

### NGINX Komutları:

`nginx -h` :
    Nginx komutları için yardım menüsünü gösterir.
`nginx -v` :
    Nginx versiyonunu gösterir
`nginx -V` :
    Nginx versiyonunu, yapılandırma bilgisini, ve ayar argümanlarını gösterir. Nginx binary'deki hangi modül yapıları olduğunu gösterir.
`nginx -t` :
    Nginx ayar dosyalarının yapılandırmasının doğruluğunu test eder.
`nginx -T` :
    Ekrana Nginx ayarlarlama test sonucunu ve doğrulanan ayarları basar. Bu komut destek bakımından kullanışlıdır.
`nginx -s signal` :
    `-s` işareti, Nginx'in ana işlemi programına bir sinyal gönderir. Yeniden açmak(reopen), tekrar yüklemek(reload), nginx'i sonlandırmak(quit) ve durdurmak(stop) gibi sinyalleri nginx'e gönderebilirsin. Stop sinyali nginx'in isteklerini tamamlar ve process'i sonlandırır. Quit ise nginx process'ini sonlandırır. Reload, *.conf dosyasındaki değişiklikler sonrası *.conf dosyasını yeniden yükler. Reopen ise log dosylarını yeniden açmak için nginx yapısına reopen sinyalini yollar.


## Nginx ile Static İçerik Sunma

Aşağıdaki `etc/nginx/conf.d/default.conf` örneği ile nginx ayarı içine yerleştirilmiş varsayılan HTTP Sunucu ayarının üzerine yazar:

```sh
server {
    listen 80 default_server;
    server_name www.example.com;

    location / {
        root /usr/share/nginx/html;
        # alias /usr/share/nginx/html;
        index index.html index.htm;
    }
}
```
