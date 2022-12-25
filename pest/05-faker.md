# Faker

我們很常使用 Faker 來建立一些假資料

Pest 有提供一個 faker plugin 讓我們可以使用，首先使用下面的方式安裝

```bash
composer require pestphp/pest-plugin-faker --dev
```

在 Pest 中的使用範例如下，假設測試能否新增一個聯絡人

```php
use function Pest\Faker\faker;

it('can store a contact', function () {
    login()->post('/contacts', [
        'first_name' => faker()->firstName,
        'last_name' => faker()->lastName,
        'email' => faker()->email,
        'phone' => faker()->e163PhoneNumber,
        'address' => 'No. 22, Fake Rd',
        'city' => 'Fake City',
        'region' => 'Fake Region',
        'country' => faker()->randomElement(['us', 'tw']),
        'postal_code' => faker()->postcode,
    ])
    ->assertRedirect('/contacts')
    ->assertSessionHas('success', 'Contact created');
})
```

如果不想額外裝套件，可以使用 `WithFaker` 這個 trait

```php
use Illuminate\Foundation\Testing\WithFaker;

uses(WithFaker::class);

it('can store a contact', function () {
    login()->post('/contacts', [
        'first_name' => $this->faker->firstName,
        'last_name' => $this->faker->lastName,
        // ...
    ])
    ->assertRedirect('/contacts')
    ->assertSessionHas('success', 'Contact created');
})
```
