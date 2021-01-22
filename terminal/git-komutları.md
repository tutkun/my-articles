## Git ile Projeleri Versiyonlama

_Git hakkında kısa bir tanıtım yapılacak..._

### Git'in Yapısı ve Kullanım Mantığı

_Git ile versiyonlamanın yapısını ve kullanım mantığı tarif edilecek..._

* Branches (master)
* Commits
* CI/CD

### Git Komutları

```sh
# Commit kayıtlarını tek satırda gösterir
git log --oneline
# Tüm değişiklikleri stage'e taşır
git add .
# dosya.txt üzerindeki değişikliği stage'e taşır
git add 'dosya/yolu/ve/dosya.txt'
# stage'e taşınan değişiklikler için açıklama belirtir
git commit -m 'commit mesajı buraya'
# Önceki commitlere geçiş
git checkout 4e2a39c3
# Yeni branch oluştur
git checkout -b chat-system
# Yeni branch'e geçiş
git branch chat-system
```

#### Git Reset Komutu

`reset` komutunun `commit`i silmesinin 2 yolu vardır. Ancak bu çok riskli bir eylemdir. Eğer ne yaptığınızdan emin değilseniz iyi düşünmelisiniz. Bir anda tüm projeye geri dönüşü olmamak üzere silebilirsiniz. :)

```sh
# commit reset ile belirtilen commite kadar siler
# Proje dizinine değişiklikler yansımaz
git reset 4e2a39c3
```

Diğeri:

```sh
# Commitleri siler, projeyi de committeki haline döndürür.
git reset 4e2a39c3 --hard
```

#### Branch Yapısı

Yeni bir branch oluşturma. Branch; şube anlamına da gelir. Kodumuz için yeni bir şube açmış oluruz. Ve yeni değişiklikleri orada yapabiliriz.

```sh
# yeni-ozellik-1 dalını ekler
git branch yeni-ozellik-1
```

Tüm branch'leri görmek için:
```sh
git branch -a
```
diyerek listeleyebiliriz.

Bir branch'den diğerine geçiş yapmak için:
```sh
git checkout yeni-ozellik-1
```
kullanılabilir. Böylece `yeni-ozellik-1` branch'i aktifleşmiş olur.

Bir branch'i silmek için
```sh
git branch -D yeni-ozellik-1
```
şeklinde kullanılır. Ve bir branch'i silmeden önce o branch'in seçili olmaması gerekir.

Eğer silinecek branch'i merge işleminden sonra silmek istiyorsak `-d` şeklinde küçük d harfi ile kullanırız.

Şimdi de bir yeni özellik daha eklemek için yeni bir branch oluşturup o branch'e geçiş yapalım:
```sh
git checkout -b yeni-ozellik-2
# bir takım değişiklikler yaptığımızı farzedip
# onları branch'e eklediğimizi düşünelim:
git add .
git commit -m "bir takım değişiklikler yaptık"
# ve master branch'e dönelim
git checkout master
```

#### Merge Commit

Yeni bir özellik için eklenen ikinci branch'ı master branch ile birleştirir. Bu işlem, yeni bir özellik eklenirken ya da bir özellik denenirken kullanılabilir.

Eğer özellikten vazgeçilirse ya da özellik bozulursa tek yapılması gereken o branch'i silmek. Yani o dal hiç mevcut olmamış olacak.

```sh
# öncelikle hangi dalda birleştirilecekse diğer dal
# o dala geçiş yapıyoruz. dal = branch
git merge yeni-ozellik-2
```

Yukarıdaki komutu, `master` branch'indeyken `yeni-ozellik-2` branch'ini `master` branch'inde birleştirmek için veriyoruz.

#### Merge Conflict ve Çözümü

Bir dosya üzerinde birden fazla kişinin işlem yapması sonucu aynı satırlarda olan değişiklikleri ya da aynı dosya ve satırlardaki ama farklı branch'lerdeki değişimleri birleştirirken, `git` bize hangi değişikliğin kullanılmak istediğini sorar.

Böyle durumlarda projemizin temel branch'indeyken diğer branch'lere birleştirme isteği yaparız. Conflict uyarısı alındığında, ilgili dosyaları açar ve gereken düzenlemeyi (son halinin nasıl olmasını istiyorsak öyle) yaparız. Ve tekrardan temel branch'imizde;

```sh
git add .
git commit -m "x.js dosyasındaki conflict düzeltildi"
```

komutlarını verip son halini temel/master branch'te uygularız. Artık dilersek diğer branch'leri silebiliriz.

#### Pull Remote - origin master

Github'daki master reposundan bilgisayarımızdaki origin repomuza verilerin çekilmesi.

```sh
git pull origin master
```

diyerek master'daki verileri origin'e çekiyoruz.

Eğer öncesinde bir tanımlama yapmadıysak;

```sh
# local'de master adında örnek şablon oluşturma
git remote add master https://git-repomuz.com/user/repo.git
```

```sh
# local branch'ları ve bağlı bulunduğu repoları listeler
git remote -v
```
