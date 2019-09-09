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

Bilgisayar yeniden başlarken `option + r` tuşu ile kurulum ekranına geçiyoruz. Ve terminal dosyası açıp `bash` 'a şu komutları giriyoruz.

```sh
-bash-3.2# cd /Volumes/Macintosh\ HD/Users/tutkun/Desktop/refind-bin-0.10.5/
-bash-3.2# ./refind-install
```
ardından size sorulan `Do you want to attempt installation [Y/N]?` sorusuna `y` cevabını verin.

#### OS-X İçinde EFI Ayarı

`finder` içinde `NO NAME` diskine girerek şu dizine gidin:
```sh
$ /EFI/BOOT/
# ardından `open with -> other` ile şu dosyaya sağ tıklayıp açın
syslinux.cfg
```

içindeki şu satırları:
```sh
include menu.cfg
default vesamenu.c32
prompt 0
timeout 0
```

şununla değiştirin:
```sh
include menu.cfg
default menu.c32
prompt 0
timeout 0
```
ve kaydedip kapatın.

- Şimdi tekrar `Disk Utility` 'yi açın.
- Apple SSD üzerine gelip `Partition (Bölümleme)` düğmesine tıklayın.
- Açılan disk pastasının altındaki `+` ikonuna tıklayarak yeni bir disk bölümü oluşturun.

Partition Bilgilerine şunları girin:
```sh
Name: Kali Linux
Format: ExFAT
Size: 25 GB
```
ardından `Apply` ve `Partition` butonlarına tıklayarak onaylayın.

Artık iki tane bölüm var:
- Macintosh HD
- Kali Linux

Şimdi bilgisayarı yeniden başlatın.
`Boot: EFI\BOOT\syslinux.cfg from XXX MiB FAT volume` kısmını seçin.

Artık Kali Linux kurulumu başladı...

Disk seçerken:
```sh
24.2 GB  f ext4  Kali Linux  /
```
şeklinde seçmeyi unutmayın.

Diğer kurulum adımlarını da tamamladıktan sonra Kali Linux açılacaktır.







