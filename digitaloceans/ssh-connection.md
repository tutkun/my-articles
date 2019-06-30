### İçerik:
1. [Sunucuda Yeni Kullanıcı Oturumu](https://gitlab.com/tutkun/reading/blob/master/digitaloceans/new-user.md)
2. [Digital Ocean Sunucu Yapılandırması](https://gitlab.com/tutkun/reading/blob/master/digitaloceans/readme.md)
3. SSH Bağlantısı
4. [Sunucunun Temel Ayarları](https://gitlab.com/tutkun/reading/blob/master/digitaloceans/sunucu-temel-ayarlari.md)


# Bu Yazıda Local, Sunucu ve GitLab Arasındaki SSH Bağlantısını Yapacağız

* `ssh-keygen` ile bir ssh anahtarı oluşturuyoruz
* `cat ~/.ssh/my_site_rsa.pub | pbcopy` ile *.pub dosyasının içeriğini hafızaya kopyalıyoruz
* digitaloceans.com adresinde ssh anahtarları kısmına yapıştırıyoruz. isim olarak da *.pub dosyasının tam adını verdim.