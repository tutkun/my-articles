## Macbook Üzerine Kali-Linux Kurulumu

Macbook üzerine bir işletim sistemi kurmak için `hdd` üzerinde en az `50GB` boş alan bulunmalı.

### Kurulum Hazırlıkları

Öncelikle flash belleği Macbook'a bağlıyoruz.

Macbook terminali üzerinde şu komut ile mevcut diskleri görüntüleyelim:
```sh
diskutil list
```

Listede USB Disk görünecektir. Bulunduğu fiziksel alanın yolunu (`/dev/diskX`) bulalım.
Daha sonra şu komutlar ile diskin bağını koparalım:
```sh
unmountDisk /dev/disk1 # disk1 örnektir. disk2 ya da disk0 da olabilir...
```

Eğer şu mesajı görüyorsanız unmount işlemi başarılı olmuştur:
```sh
Unmount of all volumes on disk1 was successful
```

Şimdi terminalde `.iso` dosyamızın bulunduğu yolu belirterek şu komutları veriyoruz:
```sh
sudo dd if=/Userss/tutkun/Desktop/kali-linux-2019.2-amd64.iso of=/dev/disk1 bs=1m
```
ve işletim sisteminizin kullanıcı şifresini girerek onaylıyoruz.

```sh
# output:
2934+1 record in
2934+1 record out
3076767744 bytes transferred in 1147.253081 secs (2681856 bytes/sec)
```
şeklinde bir çıktı görürseniz tamamdır. Şimdi terminali kapatabilirsiniz.

### rEFInd - An EFI boot manager utility

sourceforge.net üzerinden rEFInd programını indirin. @sns5694 kullanıcısının dosyası...

### Kurulum

Bilgisayar yeniden başlarken `cmd+r` tuşu ile kurulum ekranına geçiyoruz. Ve terminal dosyası açıp `bash` 'a şu komutları giriyoruz.

```sh
-bash-3.2# cd /Volumes/Macintosh\ HD/Users/tutkun/Desktop/refind-bin-0.10.5/
-bash-3.2# ./refind-install
```
ardından size sorulan `Do you want to attempt installation [Y/N]?` sorusuna `y` cevabını verin.

#### 
