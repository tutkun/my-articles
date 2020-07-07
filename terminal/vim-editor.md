# Vim Editör Kullanımı

Vim editörü karmaşık görünmesinin yanı sıra, kod yazarken gelişmiş hakimiyet sağlaması hasebiyle hızınızı arttırmanıza sebep olur. Bu da güzel bir şeydir. 😊 Eğer bu komutlara alışırsanız bir daha vazgeçemeyebilirsiniz. 😉

Bu yazıda sizlere Vim editörünün komutlarını tanıtmaya ve örnekler paylaşmaya çalışacağım. Şimdiden keyifli okumalar... 😊

## Konular
- [Vim Nedir?](#vim-nedir)
- [Diğer IDE ve Editörlerde Kullanımı](#ide-ve-editor-kullanimi)
- [Insert ve Visual Modları](#insert-ve-visual-mod)
- [Vim Komutları](#vim-komutlari)
- [Bazı Örnekler](#ornekler)

<a id="vim-nedir"></a>
## Vim Nedir?

<a id="ide-ve-editor-kullanimi"></a>
## Diğer IDE ve Editörlerde Kullanımı

<a id="insert-ve-visual-mod"></a>
## Insert ve Visual Modları

<a id="vim-komutlari"></a>
## Vim Komutları

### VISUAL Modunda Kullanılan Komutlar:

Vi içindeki çoğu komut bir tuş dizisine basar basmaz yürütülür. İki nokta üst üste (:) ile başlayan herhangi bir komut, komutu tamamlamak için <enter> tuşuna basmanızı gerektirir.

- **esc** : Esc tuşu ile insert (yazım) modundan çıkış yapıp tekrar visual moduna geçiş yapılır.
- **Yön (ok) tuşları** : İmleci, basılan yön tuşuna göre hareket ettir.
- **j, k, h, l** : İmleci aşağı (j), yukarı (k), sola (h) ve sağa (l) hareket ettirir. Yön tuşlarına benzer.
- **^** (caret işareti) : İmleci, bulunduğu satırın başına götürür.
- **$** : İmleci, bulunduğu satırın sonuna götürür
- **nG** : n'inci satıra gider (5. satıra gitmek için: **5G**)
- **G** : Son satıra gider.
- **w** : Bir sonraki kelimenin başına gider.
- **nw** : n kelime ileri gider (Örn: İmleci 2w iki kelime ileri taşır).
- **b** : Önceki kelimenin başına gider.
- **nb** : n tane kelime geri gider.
- **{** : Bir paragraf geriye gider.
- **}** : Bir paragraf ileri gider.
- **ZZ** (büyük harfler ile) : Kaydet ve çık.
- **:q!** : Son kayıttan bu yana yapılan tüm değişiklikleri silin ve çıkın.
- **:w** : Dosyayı kaydet ama çıkış yapma.
- **:wq** : Kaydet ve çıkış yap.
- **i** : Bu komut, visual (komut) modundan insert (yazım) moduna geçiş yapar. Insert; normal yazım modudur.

**Silme Komutları:**

- **x** : Her basıldığında bir karakter siler.
- **nx** : n adet karakteri siler (Örn: 3x imleçten sonraki 3 karakteri siler)
- **dd** : İmlecin bulunduğu satırı siler.
- **dn** : İmlecin bulunduğu satırdan itibaren **n** tane satırı siler. Örn: d5, imleçten sonra 5 satır siler.

**İşlemleri Geri Alma**

- **u** : Son eylemi geri alır. Geri almaya devam etmek için **u** tuşuna basmaya devam edebilirsiniz.
- **U** (büyük harf ile) : Geçerli satırdaki tüm değişiklikleri geri alır.

**NOT:** Eğer **vi** editörde **:set nu** komutunu verirseniz, satır numaralarını aktifleştirmiş olursunuz. Her satırın başında kaçıncı satır olduğu yazar. Vi'de iki nokta üst üste (:) işaretini visual modda yazdığınızda tab tuşuna basarak diğer komutlar arasında gezebilirsiniz. Ayrıca, gördüğünüz ama bilmediğiniz komutları da aşağıda belirtilen yöntem ile öğrenebilirsiniz.

Vi'ye yüzeysel bir giriş yaptık. Bu dökümanda bulamadığınız ve merak ettiğiniz daha ileri konuları şu yöntemi kullanarak bulabilirsiniz:

Kullandığınız arama motoruna **vi <aradığınız-özellik>** şeklinde yazıp aratırsanız, aradığınız şeyi bulacaksınızdır. Örn:

- vi copy and paste
- vi search and replace
- vi buffers
- vi markers
- vi ranges
- vi settings

Devam etmekten geri durmayın. Vi kullanmak başlangıçta biraz sancılı olabilir. Ama biraz pratik yaparsanız bir daha bırakmazsınız.

**NOT:** Vi, her şeyin klavyede yapıldığı bir metin editörüdür. Yazı tipini de değiştirebiliyorsanız harika ya da belki de öğrenmeyi düşünmelisiniz.

<a id="ornekler"></a>
## Bazı Örnekler
```sh
tutkun@devops: vi <dosya>
```

```sh
tutkun@devops: vi ~/yeni-dosya.txt
```

```sh
~
~
~
"yeni-dosya.txt" [New File]
```

yeni-dosya.txt içinde INSERT modu
```sh
~
~
~
- INSERT -
```

yeni-dosya.txt içinde VISUAL modu
```sh
~
~
~
- VISUAL -
```

