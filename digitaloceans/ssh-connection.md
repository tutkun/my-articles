### İçerik:
1. [Sunucuda Yeni Kullanıcı Oturumu](/digitaloceans/new-user.md)
2. [Digital Ocean Sunucu Yapılandırması](/digitaloceans/readme.md)
3. SSH Bağlantısı
4. [Sunucunun Temel Ayarları](/digitaloceans/sunucu-temel-ayarlari.md)


# Bu Yazıda Local, Sunucu ve GitLab Arasındaki SSH Bağlantısını Yapacağız

* Sunucuda Yeni Kullanıcı Oluşturma Ve Ayarlama
* SSH ile Sunucuya Bağlantı Ayarları

digitaloceans.com adresinde ssh anahtarları kısmına yapıştırıyoruz. isim olarak da *.pub dosyasının tam adını verdim.


## Sunucuda Yeni Kullanıcı Oluşturma Ve Ayarlama

Sunucuda yeni bir kullanıcı oluşturmak ve ayarlamak için aşağıdaki komutları takip edin. 
Benim oluşturacağım kullanıcı `tutkun` olacak

```sh
# tutkun adında yeni bir kullanıcı oluşturur
adduser tutkun
```

Sunucuda oluşturduğumuz kullanıcı için şifre de belirledikten sonra, `tutkun` adlı kullanıcının oturumuna geçiş yapalım:
```sh
# oturumu değiştir
sudo su tutkun
# ya da tekrar root olmak için
sudo su root
```

Bu oturumdan çıkmak için `exit`'i de kullanabilirsiniz. 

Sunucuda `root` kullanıcısını kullanarak `tutkun` kullanıcısı için yetki tanımlamasını yapınız:
```sh
# tutkun kullanıcısına admin yetkileri veriliyor
usermod -aG admin tutkun
```

Sunucuda:
```sh
# tutkun olarak oturum açıyoruz
sudo su tutkun

# vim ~/.ssh/authorized_keys
mkdir ~/.ssh
vim ~/.ssh/authorized_keys
```

ile dosyayı düzenleme modunda açıyoruz. İçerisine local'deki `sunucu_rsa.pub`'dan kopyaladığımız veriyi ekliyoruz.

Sunucuda:
```sh
# local'den *.pub içeriğini kopyalama
cat ~/.ssh/sunucu_rsa.pub | pbcopy
```
yukarıdaki `authorized_keys` dosyasına kopyaladığımız bu ssh verisini ekliyoruz.

Sunucuda:
```sh
exit # komutu ile root kullanıcısına geçiş yapıyoruz
```

Bilgisayarımızda:
```sh
# artık local bilgisayarımızdan sunucuya yeni oluşturduğumuz kullanıcı ile ssh bağlantısı yapalım
ssh -i ~/ssh/sunucu_rsa tutkun@SUNUCU_IP_ADRESI
```

Bu sayede artık kişisel bilgisayarımıza, sunucuda `tutkun` kullanıcısı için bağlantı yetkisi verdik.
Sunucuda `tutkun` kullanıcısı aktifken şu komutu veriyoruz:
```sh
# ssh ayar dosyası
sudo vim /etc/ssh/sshd_config
```

Bu dosyada iki şeyi `no` olarak değiştiriyoruz. Bunlar:
```sh
PermitRootLogin yes # no olacak
PasswordAuthentication yes # no olacak
```

Sunucuda:
```sh
# exit ile tutkun kullanıcısından çıkış yapıp root kullanıcısına geçiyoruz
exit

# ve ssh'ı yeniden başlatıyoruz
sudo service ssh restart
```

Ardından bilgisayarımızdan tekrar:
```sh
ssh root@IP_ADRESIMIZ
```

```sh
# tekrar bu şekilde kullanıcıya identifier parametresi ile giriş yaptırıyoruz
ssh -i ~/ssh/id_rsa tutkun@SUNUCU_IP_ADRESI
```


## SSH ile Sunucuya Bağlantı Ayarları

Bilgisayarınızda:
```sh
# ssh-keygen ile bir ssh oluşturuyoruz (tutkun adı ile: dosya sonundaki yorum sanırım)
ssh-keygen -t rsa -b 4096 -C "tutkun"

# Gerekli adımlardan sonra ssh için parola sorulur. 
# Bu parola bilgisayarınızdaki ssh'ı korumak içindir. 
# Sunucu şifresi değildir.
# Bu adımdan sonra ssh-key'iniz oluşturuldu
```

Şimdi bilgisayarınızda oluşturulan bu `ssh`ın `*.pub` uzantılı dosyasının içeriğini geçiçi hafızaya alarak sunucunuzdaki `authorized_keys` kısmına ekleyeceğiz:
```sh
cat ~/.ssh/id_rsa.pub | pbcopy # pbcopy mac içindir
```

Sunucunuzda:
```sh
sudo vim ~/.ssh/authorized_keys
# dosyasının içine bilgisayarınızdan kopyaladığınız ssh.pub'ı yapıştıryoruz
exit
```
ile sunucudan çıkabiliriz.

Bilgisayarınızdan ssh ile bağlanabiliyor muyuz bakalım:
```sh
# root ve _ip_address_ sizde farklı olacaktır
ssh -i ~/.ssh/id_rsa root@_ip_address_
```

**NOT:** Artık sunucuya `ssh` ile doğrudan bağlanabilir ve `PHP`, `Nginx`, `MySQL` ve `git` gibi uygulamaları kurabiliriz. :)


## Devamı:
[Video Serisi](https://bitfumes.com/courses/laravel/deploy-laravel-on-digital-ocean/tutorial-2)
[Videolu Anlatım'da Kalınan Yer](https://youtu.be/T7WinEDS7e4?t=535)
