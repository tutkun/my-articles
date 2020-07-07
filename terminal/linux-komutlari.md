# LINUX İLE İLGİLİ TÜM KOMUTLAR

Burada Linux tabanlı shell scriptlerinin nasıl çalıştığını göreceğiz. Hangi komutların nasıl kullanıldığını ve hangi ek özelliklerle nasıl faydalanıldığını göreceğiz.

[Şurada](https://www.geeksforgeeks.org/array-basics-shell-scripting-set-1) da güzel bir kaynak mevcut.

Bu anlatımların benzerleri [şuradaki youtube](https://www.youtube.com/playlist?list=PLSg_-k7KzeO-9oYlhrPeuzQJ5NuP5FAvv) sayfasında da mevcut.

## Konular
- `echo` Komutu
- `if` Komutu
- `if-else` Komutu
- `if-elif` Komutu
- Değişken tanımlama
- `case` Komutu
- `while` Komutu
- `for` Komutu
- `sort` Komutu
- `uniq` Komutu
- Fonksiyon tanımlama

### ECHO Komutu
Bir şeyi ekrana basmak için:
```sh
    echo "Merhaba Kıymetlimissss! 😅"
```

### IF Komutu
`bu` ile `buna` adlı iki parametre birbinine eşit değilse:
```sh
if [[ "bu" -ne "buna" ]]; then
    echo "Parametreler eşit değil: -ne (not equals)"
fi

```

### IF-ELSE Komutu
Burada da else ile diğer durumu göreceğiz:
```sh
if [[ $degisken_degeri -eq "buna eşit" ]]; then
    echo "Parametreler eşit: -eq (equals)"
else
    echo "Parametreler eşit değil: -ne (not equals)"
fi
```
Yukarıda `[[` ile `$degisken_degeri` arasında bir boşluk olması önemlidir. Aslında her komut arasında bir boşluk olmalı.

### IF-ELIF Komutu
Burada da if-elif-else durumunu göreceğiz:
```sh
if [[ "bu" -eq "buna" ]]; then
    echo "Parametreler eşit: -eq (equals)"
elif [[ 4 -lt 6 ]]; then
    echo "4 Küçük ise 6'dan: -lt (less then)"
fi

```


### IF-ELIF-ELSE Komutu
Burada da if-elif-else durumunu göreceğiz:
```sh
if [[ "bu" == "buna" ]]; then
    echo "Parametreler eşit: == (equals)"
elif [[ 4 -gt 6 ]]; then
    echo "4 Büyük ise 6'dan: -gt (great then)"
else
    echo "İkisi de değil"
fi

```


### Değişken Tanımlama
Şimdi de bir değişkenin nasıl tanımlanıp kullanıldığını göreceğiz:
```sh
# tanımlarken $ işareti yok:
isim="Ahmet"

# Kullanırken $ ile başlamalı:
if [[ $isim == "Samet" ]]; then
    echo "Hoşgeldin $isim"
else
    echo "Merhaba, tanışıyor muyuz?"
fi

```



### CASE Komutu
Şimdi de `case` komutunun nasıl kullanıldığını göreceğiz:
```sh
# tanımlarken $ işareti yok:
isim="Ahmet"

# Kullanırken $ ile başlamalı:
case $isim in
    "Samet"|"samet" )
        echo "Hoşgeldin $isim"
    ;;
    * )
        echo "Üzgünüm, tanıyamadım."
    ;;
esac

```

### WHILE Döngü Komutu
While ile döngü nasıl yapılır bakalım:
```sh
sayi=0
while [ $sayi -lt 10 ]; do
    echo "Sayı: $sayi"
    
    # bir değişkene yeniden değer atamak için $ işaretini başından alıyoruz:
    sayi=$(( $sayi + 1 ))
    
    # Ya da diğer bir kullanım şekli:
    # sayi=`expr $sayi + 1`
done

```

While ile bir dosya içeriğini okumak için şöyle bir şey yapılır:
```sh
dosya="/etc/passwd"
while IFS=: read -r satirdaki_sutun1 satirdaki_sutun2 satirdaki_sutun3 satirdaki_sutun4 satirdaki_sutun5 satirdaki_sutun6 satirdaki_sutun7 satirdaki_sutun8 satirdaki_sutun9
do
    echo "Okunan satırdaki kullanıcılar -> Kullanıcı: $satirdaki_sutun1"
done < $dosya

```
tabii ki daha spesifik işlemler de yapılabilir. Mesela `done < $dosya` yerinde `$(ls -l | awk '{print $0}')` şeklinde bir dizin içindeki dosya/dizinler listelenip ilk sütuna denk gelenler alınabilir.

### FOR Komutu
for döngüsünün çalışma şekli

```sh
for i in {1..10} # $(seq 1 10) şeklinde de olabilir
do
    echo "$i . değer"
done

```

### Sort Kullanımı
Bir dosya içindeki metni satırlara göre sıralı almak:
```sh
# önce bir dosya oluşturalım:
touch metin.txt

# şimdi dosyaya şu metni satırlar halinde girelim:
echo """
birinci
ikinci
üçüncü
dördüncü
beşinci
altıncı
altıncı
beşinci
""" >> metin.txt

# şimdi cat ile ekrana dosya içeriğini alıp sort ile sıralı şekilde basalım:
cat metin.txt | sort

```

### Unique Kullanımı
Bir metin içindeki satırları `sort` ile alfabetik olarak sıralayıp aynı olan satırları `uniq` ile çıkartıp ekranda gösteriyoruz:
```sh
cat metin.txt | sort | uniq

# artık dosyayı silebiliriz:
rm -f metin.txt

```

### Fonksiyon Tanımlama
Aşağıda kullanılmak üzere bir fonksiyon tanımlayalım:
```sh
cikartma_islemi () {
    # fonksiyona girilen parametreler $1 'den başlar. Kaç tane girildiyse o kadara gider...
    parametreler=$(( $1 + $2 ))
    echo "Merhaba, parametreler: $parametreler"
}

# fonksiyona 2 parametre gönderip çıkartma işlemini yapıyoruz:
cikartma_islemi 120 20

```

_Fırsat buldukça güncellerim..._