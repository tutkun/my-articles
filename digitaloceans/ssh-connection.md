### İçerik:
1. [Sunucuda Yeni Kullanıcı Oturumu](https://gitlab.com/tutkun/reading/blob/master/digitaloceans/new-user.md)
2. [Digital Ocean Sunucu Yapılandırması](https://gitlab.com/tutkun/reading/blob/master/digitaloceans/readme.md)
3. SSH Bağlantısı
4. [Sunucunun Temel Ayarları](https://gitlab.com/tutkun/reading/blob/master/digitaloceans/sunucu-temel-ayarlari.md)


# Bu Yazıda Local, Sunucu ve GitLab Arasındaki SSH Bağlantısını Yapacağız

* `ssh-keygen` ile bir ssh anahtarı oluşturuyoruz
* `cat ~/.ssh/my_site_rsa.pub | pbcopy` ile *.pub dosyasının içeriğini hafızaya kopyalıyoruz
* digitaloceans.com adresinde ssh anahtarları kısmına yapıştırıyoruz. isim olarak da *.pub dosyasının tam adını verdim.

## Yeni Kullanıcı Oluşturma Ve Ayarlama

Yeni bir kullanıcı oluşturmak ve ayarlamak için aşağıdaki komutları takip edin

```sh
# tutkun adında yeni bir kullanıcı oluşturur
adduser tutkun
```

Şifre bilgilerini de girdikten sonra şu komut ile `tutkun` adlı kullanıcının oturumuna geçiş yapıyoruz

```sh
# oturumu değiştir
sudo su tutkun
```

Bu oturumdan çıkmak için `exit` kullanabilirsiniz. 

`root` kullanıcısını kullanarak `tutkun` kullanıcısı için yetki tanımlamasını yapınız:

```sh
# tutkun kullanıcısına admin yetkileri veriliyor
usermod -aG admin tutkun
```

Ayarların yapılması:

```sh
# tutkun olarak oturum açıyoruz
sudo su tutkun

# vim ~/.ssh/authorized_keys
mkdir ~/.ssh
vim ~/.ssh/authorized_keys
```

ile dosyayı düzenleme modunda açıyoruz. İçerisine local'deki `sunucu_rsa.pub`'dan kopyaladığımız veriyi ekliyoruz.

```sh
# local'den *.pub içeriğini kopyalama
cat ~/.ssh/sunucu_rsa.pub | pbcopy
```
yukarıdaki `authorized_keys` dosyasına kopyaladığımız bu ssh verisini ekliyoruz.

```sh
exit # komutu ile root kullanıcısına geçiş yapıyoruz
```

```sh
# artık local bilgisayarımızdan sunucuya yeni oluşturduğumuz kullanıcı ile ssh bağlantısı yapalım
ssh -i ~/ssh/sunucu_rsa tutkun@SUNUCU_IP_ADRESI
```

Bu sayede artık kişisel bilgisayarımıza `tutkun` kullanıcısı için bağlantı yetkisi verdik.
`tutkun` kullanıcısı aktifken şu komutu veriyoruz:
```sh
# ssh ayar dosyası
sudo vim /etc/ssh/sshd_config
```
Bu dosyada iki şeyi `no` olarak değiştiriyoruz. Bunlardan biri `PermitRootLogin yes` diğeri de `PasswordAuthentication yes` olan kısımları `no` yaparak dosyayı kaydedip çıkıyoruz.

```sh
# exit ile tutkun kullanıcısından çıkış yapıp root kullanıcısına geçiyoruz
exit

# ve ssh'ı yeniden başlatıyoruz
sudo service ssh restart
```

Ardından tekrar;
```sh
ssh root@IP_ADRESIMIZ
```

```sh
# tekrar bu şekilde kullanıcıya yetkili giriş yaptırıyoruz
ssh -i ~/ssh/sunucu_rsa tutkun@SUNUCU_IP_ADRESI
```

## Devamı:
[Video Serisi](https://bitfumes.com/courses/laravel/deploy-laravel-on-digital-ocean/tutorial-2)
[Videolu Anlatım'da Kalınan Yer](https://youtu.be/T7WinEDS7e4?t=535)