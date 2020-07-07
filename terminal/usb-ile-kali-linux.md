# Kali Linux Kurulumu İçin USB'nin Hazırlanması
(Şurada)[https://www.youtube.com/watch?v=wdBFXRzmHvw&index=2&list=LL0n3KKByS4BkfKTbAF7iY5Q&t=0s&ab_channel=DerangedSociety] anlatıldığı şekli ile yapılabilir.

## Kali Linux'un ISO Dosyasından USB'ye mount

Kali linux iso dosyasını kullanıcı dizinine `cp` komutu ile kopyalıyoruz:
```sh
cp ./kali-linux-x64.iso $HOME/kali-linux-x64.iso
```

Kullanıcı dizinine geçiş yapıyoruz:
```sh
cd ~/
// veya
cd $HOME
```

Mac'teki disklerin volume'lerini listeliyoruz:
```sh
diskutil list
```

Aşağıdaki komut ile örnek olarak `/dev/disk2` diskini `unmount` ediyoruz:
```sh
diskutil unmountDisk /dev/disk2
```

Aşağıdaki komutu verdiğinizde biraz uzun süre beklemeniz gerekebilir.
Bu komut iso dosyasını diske yazmaya başlamış olacak.
```sh
sudo dd if=kali-linux-2018.3a-amd64.iso of=/dev/disk2 bs=1m
```

Alacağınız yanıt şu olacaktır:
```sh
2934+1 records in
2934+1 records out
8934293422 bytes transferred in 872.23423423 secs (7243334 bytes/sec)
```

Artık dsiki çıkartıp format atılacak bilgisayara takabiliriz.

## Windows 10 Kurulumu için ise

(Şurada)[https://appleinsider.com/articles/18/01/18/tips-how-to-make-windows-10-install-media-on-macos-high-sierra] belirtilen adresteki gibi işlemler uygulanarak windows 10 usb belleği yapılabilir.


