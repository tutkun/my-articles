# VueJS Hakkında

### Tüm Vue Dosyalarından Erişim

Bazen; API url linklerinin ve yetkilendirilmiş kullanıcıyı tutan değişkenin tüm Vue bileşenlerinden erişimini sağlamak için şunlar yapılabilir:

Api Url bilgileri için **api.js** dosyası:
```js
// routes/api.php içindeki linkleri referans alır.
const urls = {
	'api': {
		'me': '/api/me',
		'v1': {
			'search': '/api/v1/search',
			'messages': '/api/v1/messages',
			'roles': '/api/v1/roles',
			'permissions': '/api/v1/permissions',
		},
	},
	'logout': '/logout',
	
	install: function(){
		Object.defineProperty(Vue.prototype, '$urls', {
			get () { return urls }
		})
	}
};
// 
export default urls;
```

Yukarıdaki dosyayı eğer *app.js* içinde `require('vue')`'dan sonra import eder ve `Vue.use(urls)` gibi global olarak kullanılacağını beyan edersek; artık tüm vue bileşenleri üzerinden `this.$urls` ya da html kısmında `$urls` şeklinde yukarıda belirtilen içeriğe erişebiliriz.

## Bir Auth yapısı

Yine yukarıdaki mantıkla oluşturulacak global bir auth yapısı ile giriş yapan kullanıcının bilgilerine her noktadan erişim sağlanır.

**Örnek dosya:**
```js
const auth = {
	
	getUser() {
		return sessionStorage.getItem('me') ? JSON.parse(sessionStorage.getItem('me')) : null;
	},
	
	setUser(data) {
		JSON.stringify(data);
    },
	
	install: function(){
		Object.defineProperty(Vue.prototype, '$auth', {
			get () { return auth }
		})
	}
	
};

export default auth;
```

**Not** Burada önemli olan kısım; `Object.defineProperty`'nin 2. parametresidir. Burada, bileşenin globalde hangi ad ile çağırılacağını belirtiyoruz.