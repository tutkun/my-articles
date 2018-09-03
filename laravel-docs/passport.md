# Passport Kütüphanesi Hakkında

`axios` ile kullanırken bir iki dikkat edilmesi gereken husus var. Mesela `401` hataları alıyorsan
ve sunucu tarafında yetkilendirmeler yapıldıysa, sorunun ajax kısmında olduğunu düşünüyorsan bakacağın ilk yer
`bootstrap.js` içindeki `axios.default.headers.common` parametresine girilen değerlerdir. Normalde header'a `Authorization` eklemek gerekir. Fakat `app\Http\Kernel.php` için `\Laravel\Passport\Http\Middleware\CreateFreshApiToken::class,` eklenmesi ile durum düzeltilir. Çünkü bu sınıf, sisteme normal şekilde giriş yapan kullanıcının session işlemlerini `laravel_token` aracılığı ile sunucu tarafında gerçekleştirir.