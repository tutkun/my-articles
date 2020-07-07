*ÖNEMLİ NOT:* Bu tanıtımın `şu an` için ne derece geçerli ve güncel olduğunu bilmiyorum. Kontrol etmedim. Kullanacak olan kişilerin ilgili bağlantıdan kontrol etmelerini öneriririm. Kullanmamama rağmen çeşitlilik olması açısından çok eski olan bu makaleyi silmiyorum.

## Messenger

Güzel bir mesajlaşma sistemine sahip composer modülü.
[gerardojbaez/messenger](https://github.com/gerardojbaez/messenger) github adresinden inceleyebilirsiniz.

### Gereksinimler

* Laravel 5.x
* PHP >=5.5


### Composer Ayarı
```json
{
    "require": {
        "gerardojbaez/messenger": "~1.0"
    }
}
```

Composer'ı çalıştır
```bash
composer install
```


### Service Provider Ayarı

```php
'providers' => [
	/**
	 * Third Party Service Providers...
	 */
	Gerardojbaez\Messenger\MessengerServiceProvider::class,
]
```

```php
'aliases' => [
	'Messenger' => Gerardojbaez\Messenger\Facades\Messenger::class,
]
```

### Config Dosyası ve Migrations

```php
php artisan vendor:publish --provider="Gerardojbaez\Messenger\MessengerServiceProvider"
```

```bash
php artisan migrate
```

### Trait'ler ve Contract'lar

```php
<?php

namespace App;

use Laravel\Passport\HasApiTokens;
use Zizaco\Entrust\Traits\EntrustUserTrait;
use Gerardojbaez\Messenger\Contracts\MessageableInterface;
use Gerardojbaez\Messenger\Traits\Messageable;

use Illuminate\Notifications\Notifiable;
use Illuminate\Foundation\Auth\User as Authenticatable;
use Illuminate\Database\Eloquent\SoftDeletes;

class User extends Authenticatable implements MessageableInterface
{

    use HasApiTokens, Messageable, SoftDeletes, Notifiable;

    /**
     * The attributes that should be mutated to dates.
     *
     * @var array
     */
    protected $dates = ['deleted_at'];

}
```

### Kullanım

*Bir kullanıcıya mesaj gönderimi*
```php

// Import the facade
use Messenger;

// $user kullanıcısının $user2 kullanıcısına mesaj göndermesi
Messenger::from($user)->to($user2)->message('Hey!')->send();

// $user tarafından birden fazla kişiye mesaj göndermek için; diğer user `id`lerini array olarak gönder
Messenger::from($user)->to([1,2,3,4,5])->message('Hey!')->send();

// $user tarafından $thread'e mesaj gönderimi
Messenger::from($user)->to($thread)->message("That's awesome!")->send();

// okunmamış mesajların sayısı
echo $user->unreadMessagesCount;

// $user'a ait threadlerden ilkinin okunmayan mesajlarının sayısı
echo $user->threads->first()->unreadMessagesCount;

// Thread'i okundu olarak işaretle
$user->markThreadAsRead($thread_id);
```

### Thread Dinamik Özellikleri

Bunlar; veritabanında tutulmayan, mesajların içeriğine göre oluşturulup gösterilendir
Örn; konu başlıklarının tek başına bir başlığı yok. Messenger katılımcı listesine göre oluşturacak.

*Attributes:*
* `$thread->title` thread'in başlığı
* `$thread->creator` thread oluşturucuyu almak için
* `$thread->lastMessage` thread'deki en son mesajı almak için

### Kullanıcı threadlerini gösterme

*Controller*
```php
public function index()
{
    // Eager Loading - this helps prevent hitting the database more than the necessary.
    // Eager Yükleme ile veritabanından daha fazla tabloyu çekebiliriz:
    $this->user->load('threads.messages.sender');

    // views/messages/index.php'ye $threads olarak $this->user->threads'i aktarmak
    return view('messages.index', [
        'threads' => $this->user->threads
    ]);
}
```

*View*
```html
<div class="panel panel-default">
    <div class="list-group">
        @if($threads->count() > 0)
            @foreach($threads as $thread)
                @if($thread->lastMessage)
                    <a href="#" class="list-group-item">
                        <div class="clearfix">
                            <div class="pull-left">
                                <span class="h5">{{ $thread->title }}</span>
                                @if($thread->unreadMessagesCount > 0)
                                    <span class="label label-success">{!! $thread->unreadMessagesCount !!}</span>
                                @endif
                            </div>
                            <span class="text-muted pull-right">{{ $thread->lastMessage->created_at->diffForHumans() }}</span>
                        </div>
                        <p class="text-muted no-margin">{{ str_limit($thread->lastMessage->body, 35) }}</p>
                    </a>
                @endif
            @endforeach
        @endif
    </div>
</div>
```

### Sahip Olunan Modeller

Messenger 3 modele sahip:

```php
use Gerardojbaez\Messenger\Models\Message;
use Gerardojbaez\Messenger\Models\MessageThread;
use Gerardojbaez\Messenger\Models\MessageThreadParticipant;
```

Bunları normal şekilde kullanabilirsiniz. Daha fazla bilgi için her modele ve 
`Gerardojbaez\Messenger\Traits\Messageable` özelliklerine göz atın.
