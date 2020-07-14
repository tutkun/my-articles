# Algoritmalar

Bu yazıda, bazı matematiksel algoritmaları açıklayacağım. Elbette bazı kısımlardaki kodların daha da kısa ve performanslı kullanımları var. Ancak buradaki amacım, kodu anlaşılır kılma çabasıdır.

### Bir Sayının -1 ile Çarpımı

Bir sayının işaretini tersine çevirmek istiyorsak, o sayıyı `-1` ile çarparız.

```py
x = 5
# - ile -'nin çarpımı +'dır. yani -'ler birbirini götürür
sayi = x * -1
print(sayi)
# Sonuç: -5
```

### Tek mi, Çift mi

Bir sayının tek mi, çift mi olduğunu anlamak için: o sayının 2'ye bölümünden kalanın 0 mı, 1 mi olduğunu bilmek gerekir.

```py
sayı = 10
mod = sayı % 2 # %, programlamada bölme işareti değil, mod işaretidir.
print(mod === 1 ? 'Tek' : 'Çift')
```

### 1'den n'e Kadar Olan Çift Sayıların Toplamı

n tane çift sayının toplamını hesaplamak

```py
n = 10
toplam = 0
for sayi in range(1, n):
    if sayi % 2 === 0:
        toplam += sayi
print(toplam)
```

### Bir Sayının Rakamlarının Toplamı

Modüler aritmatik ve bölme işlemlerini kullanarak bir sayının rakamlarının toplamını bulabiliriz.

```py
rakam = 0
toplam = 0
while(n > 0):
    rakam = n % 10
    toplam += rakam
    n = n / 10
print(toplam)
```

### Birden n'e Kadar Olan Sayıların Toplamı

##### Diğer adıyla Ardışık sayıların toplamı

Bir sayının, kendisi ile kendisinden bir fazlasının çarpımı; 1'den o sayıya kadar olan tüm sayıların toplamını verir.

```py
n = int(input('Bir sayı giriniz: '))
sayi = n * (n+1)
print( "{1} sayısı, 1'den {0}'a kadar olan sayıların toplamıdır.".format(n, sayi) )
```

### Birden n'e Kadar Olan Tek Sayıların Toplamı

1'den belirtilen bir sayıya (n) kadar olan, tekil sayıların toplamını hesaplayalım.

```py
n = int(input('Bir sayı giriniz: '))
for i in range(1, n):
    if i % 2 == 1:
        sayi = 2*n-1

print( "{1} sayısı, 1'den {0} sayısına kadar olan TEK sayıların toplamıdır.".format(n, sayi) )
```

### N'e Kadar Olan Çift Sayıların Toplamı

N'e kadar olan çift sayıların toplamını bulacağımız formül.

```py
n = int(input('Bir sayı girin: ')) / 2
print( n*(n + 1) )
```

### Aralıklı Çift Sayıların Toplamı

X ile Y arasındaki sayıların toplamını bulan formül

```py
t1 = int(input('Başlangıç Sayısı: ')) / 2
t2 = int(input('Bitiş Sayısı: ')) / 2

aralikliCift = (t1*(t1+1) - t2*(t2+1))

print(aralikliCift)
```

### Çemberin Çevresini ve Alanını Bulma

Sonsuz sayıdaki noktaların, bir merkez noktasına sabit uzaklıkta olmak şartıyla yan yana dizilmesiyle çember oluşur.
Merkez noktanın çevresine dizilen bu noktaların oluşturduğu dairenin uzunluğunun, çemberin yarıçapına bölümü bize PI sayısını verir.

##### Çemberin çevresi

```py
import math
# yarıçap
r = 5
# çevreyi bulmak için
cevre = 2 * math.pi * r

print("Çemberin çevresi {0} birimdir.".format(cevre))
```

##### Çemberin alanı

```py
import math
# 5 birim yarıçap
r = 5
# çevreyi bulmak için
cevre = 2 * math.pi * r
# çemberin alanı için
alan = math.pi * r * r
# çapı bulmak için
d = cevre / math.pi

print("Çemberin alanı {0} birimdir.".format(alan))
```

##### Çemberin belirtilen açısı kadar alanını hesaplama

Merkez nokta olarak adlandırdığımız etrafında, noktaya olan uzaklıkları eşit, birbirlerine ardışık diğer noktalar ile oluşur.

```py
import math
# 5 birim yarıçap
r = int(input('Bir Yarıçap Gir: '))
aci = int(input('Bir Yarıçap Gir: '))
# çevreyi bulmak için
cevre = 2 * math.pi * r
# çemberin açı kadar olan alanı için
alan = math.pi * r * r * (aci/360)
# çapı bulmak için
d = cevre / math.pi

print("Çemberin {0} derece olan açısının alanı {0} birimdir.".format(alan))
```

### Bir Dizideki En Büyük ve En Küçük Elemanlar

Bir sayı dizisindeki en küçük ve en büyük sayıyı bulmak için bir döngüden yararlanabiliriz.
Bu, işlemin mantığını kavramak için olsa da performans açısından daha uygun yöntemi de vereceğiz.

```py
from random import random, rangex
adet = int(input('Dizi\'deki eleman sayısı: '))

dizi = rangex(1, int(random() * adet))
for s in dizi:
    print(s)
```

### Aritmetik Ortalama Hesabı

Aritmetik ortalama; bir sayı dizisindeki öğelerin sayı değerleri toplamı, bu sayı dizisinin eleman sayısına bölümüdür.

```py
def aritmetikOrt(sayi_dizisi = []):
    toplam = 0
    for i in sayi_dizisi:
        toplam += i
    return toplam / len(sayi_dizisi)

print(aritmetikOrt([2,4,6,8])) # sonuç 5.0
```

### Standart Sapma Hesabı

```py
import math

x = [3,5,14,9]
# aritmetik ortalama hesabı yukarıda
aOrt = aritmetikOrt(x)

t = sum([math.pow(i-aOrt, 2) for i in x])
std = t / (len(x) - 1)
std = math.sqrt(std)

return std
```

### Tek ve Çift Sayıların Adeti

```py
sayilar = [2,4,5,6,20]
tek = []
cift = []
for sayi in sayilar:
    if sayi%2:
        tek.append(sayi)
    else:
        cift.append(sayi)
print("Tek sayılar: {0}".format([x for x in tek]))
print("Çift sayılar: {0}".format([x for x in cift]))
```

### Char ve Rakamların İşlenmesi

```py
from random import *

def l(x=1, y=1500, r=10):
    i = 1
    v = ""
    for k in range(x, y):
        nk = chr(k)
        if nk == "\n" or nk == "\t":
            continue
        if i % r:
            v += "{0}({1})\t\t".format(nk, k)
        else:
            v += "\n{0}({1})\t\t".format(nk, k)
        i += 1
    print(v)
```

### Üst Alma Fonksiyonu

Bir sayının kuvveti; o sayının kendisi ile kaç kez çarpılacağını ifade eder.
Örn: 2 sayısının 3'üncü kuvveti, 2'nin kendisi ile 3 kez çarpımını ifade eder.

```py
from random import *

def ust_alma(taban=2, kuvvet=3):
    s = 1
    for i in range(s, kuvvet+1):
        s *= taban
    print(s)
    # sonuç: 8
```

### Bir Asal Sayı Bulma

Bir sayı, kendisinden daha küçük bir sayıya bölünemezse, o sayı **asal sayıdır**.
Aşağıdaki örnekte, 100'e kadar olan asal sayıları bulalım.

```py
from random import *
x = 100

def asal_sayi(x):
    bolen = []
    status = False
    for asal in range(1, x):
        if x % asal == 0:
            bolen.append(asal)
    for s in bolen:
        if s < x and s != 1 and s != None:
            status = True
    if not status:
        print("Sayı asal: ", bolen)
    else:
        print("Sayı asal değil: ", bolen)
```

### Bir Sayının Asal Çarpanlarını Bulma

Bir sayının, ikiye bölümünden kalan sayı 0 olana dek 2'ye bölünmesini ve eğer kalan sayı 2'ye bölünmüyorsa, bölen bir arttırılır. Böylece yeni bölen değeriyle bölme işlemi tekrar eder. Bölünen 0 oluncaya dek bu işlem tekrarlanır.

```py
def asal_carpanlar(sayi):
    i = 2
    c = []
    while(sayi > 1):
        if not sayi % i:
            sayi = int(sayi / i)
            c.append(sayi)
        else:
            i+=1
    return c
```

### Bir Sayının Asal Çarpanları Toplamı

Bir sayının asal çarpanları toplamı

```py
x = 10

def carpanlari_toplami(x):
    carpanlari_toplami = 0
    for i in asal_carpanlar(x):
        carpanlari_toplami += i
    print(carpanlari_toplami)
```

### Bir Sayının Asal Çarpanlarının Çarpımı

Bir sayının asal çarpanlarının birbiri ile çarpımıdır. Tekrarlayan çarpanlar ele alınmaz.
Örn: [2, 2, 3, 3, 5] gibi tekrar eden sayılar.

```py
x = 10

def asal_carpanlarinin_carpimi(x):
    carpanlarinin_carpimi = 1
    for i in asal_carpanlar(x):
        carpanlarinin_carpimi *= i
    print(carpanlarinin_carpimi)
```

### OKEK (Ortak Katların En Küçüğü) Hesaplaması

Belirtilen iki sayının, **Ortak Katlarının En Küçüğü**nün bulunmasını amaçlar.

```py
def okek(s1, s2):
    o_sayi = 1
    while (s1 != 1 and s2 != 1):
        bolen = 2
        i = 1
        buyukSayi = s2 if s1 < s2 else s1
        while (i <= buyukSayi):
            if s1 % bolen == 0 or s2 % bolen == 0:
                o_sayi *= bolen
                if s1 % bolen == 0:
                    s1 /= bolen
                if s2 % bolen == 0:
                    s2 /= bolen
            else:
                bolen += 1
            i += 1
    return o_sayi
```

### OBEB (Ortak Bölenlerin En Büyüğü) Hesaplaması

1 sayısından büyük olan eldeki iki ayrı sayının, en büyük ortak böleninin hesaplanmasını ifade eder.
Bölen sayısının, 2'den başlayarak, eldeki sayılardan en büyüğüne ulaşana kadar bölme işlemine devam edilir.

```py
def obeb(s1, s2):
    s = 1
    bolen = 2
    while(s1!=1 and s2!=1):
        i = 2
        ikiSayidanEnBuyugu = s2 if s1 < s2 else s1
        while(i < ikiSayidanEnBuyugu):
            if s1%bolen==0 or s2%bolen==0:
                if s1%bolen==0 and s2%bolen==0:
                    s *= bolen
                if s1%bolen==0:
                    s1 /= bolen
                if s2%bolen==0:
                    s2 /= bolen
            else:
                bolen+=1
            i+=1
    return s
```

### Faktöryel Hesabı

Belirtilen sayıdan 1 sayısına kadar ardışık olan sayıların birbiri ile olan çarpımının sonucudur.
Örn: 5! için;
5x4x3x2x1 = 120

```py
def f(x):
    f = 1
    for i in range(1, x+1):
        f *= i
    return f
```

### Aralıklı Faktöryel Hesabı

İki sayı arasındaki faktöryellerin uzunluğu

```py
def fa(aralik, faktoryel):
    f = 1
    t = 0
    sayilar = range(aralik, faktoryel+1)
    for i in sayilar:
        f *= i
        t += f
        print("{0}! = {1}".format(i,f))
    ort = t/len(sayilar)
    return "Toplamın Ortalaması: {0}".format(ort)
```

### İkili Tabandan Sayı Okuma

```py
def karar(n):
    i = 0
    sayi = 0
    kontrol = True
    for s in str(n):
        if s in [0,1]:
            pass
        i = int(s)
        if 0 is not i and 1 is not i:
            kontrol = False
    if kontrol:
        print("Sayı içerisinde 0 ya da 1 değerlerinden oluşuyor: {0}".format(n))

karar(190)
karar(101)
karar(1110)

# burada bazı hatalar var.
# karar(-1110)
# karar(010)
```

### Skaler Matris

Asal köşegenler üzerinde olan değerler birbirine eşit olmak zorunda.
Örn: 3x3 bir matrisin; 1x1, 2x2 ve 3x3 kısımları aynı değere işaret etmeliler.

```py
from random import *

def skaler_matris(matris=3, scaler=5):
    for i in range(0, matris):
        for j in range(0, matris):
            if i == j:
                m.append(scaler)
            else:
                m.append(0)
    return m
```
