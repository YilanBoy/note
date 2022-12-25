# Expectations

簡單介紹 Pest 的 `expect()` 方法

## Expect API

一般我們用來斷定一個值時，通常會使用 PHPUnit 提供的各種 `assert` 方法

```php
$this->assertEqual($name, 'Allen');
```

Pest 提供一種更為優雅且更為清楚的斷定 (assert) 值類型 API：`expect()`

```php
expect(true)->toBeTrue();
```

使用剛剛的新增聯絡人範例，看看 `expect()` 提供哪些方法

```php
use function Pest\Faker\faker;

it('can store a contact', function () {
    login()->post('/contacts', [
        'first_name' => faker()->firstName,
        'last_name' => faker()->lastName,
        'email' => faker()->email,
        'phone' => faker()->e164PhoneNumber,
        'address' => 'No. 23, Fake Rd',
        'city' => 'Fake City',
        'region' => 'Fake Region',
        'country' => faker()->randomElement(['us', 'tw']),
        'postal_code' => faker()->postcode,
    ])
    ->assertRedirect('/contacts')
    ->assertSessionHas('success', 'Contact created');

    $contact = Contact::latest()->first();

    expect($contact->first_name)->toBeString(); // pass
    expect($contact->first_name)->toBeEmpty(); // fail
    expect($contact->first_name)->toBeString()->not->toBeEmpty(); // pass
    expect($contact->email)->toBeString()->toContain('@'); // pass
    expect($contact->phone)->toBeString()->toStartWith('+'); // pass
    expect($contact->address)->toBe('No. 23, Fake Rd'); // pass
    expect($contact->country)->toBeIn(['us', 'tw']); // pass
})

```

## 定義自己的 Expect 的斷定方法

在之前的 `expect()` 範例中，我們使用下方的方式來判斷一個信箱地址的格式是否正確

```php
expect($contact->email)->toBeString()->toContain('@');
```

但這個方式直觀來看，很難第一眼了解這麼做的目的是什麼

Pest 提供為 `expect()` 客製方法的功能，我們可以在 `tests/Pest.php` 中為 `expect()` 加上新的方法

```php
expect()->extend('toBeEmailAddress', function () {
    expect($this->value)->toBeString()->toContain('@');
});
```

之後我們就可以在測試中使用 `toBeEmailAddress()`

```php
expect($contact->email)->toBeEmailAddress();
```

不過上面的判斷方法其實還有很多漏洞，假設我們填寫一個字串

```text
fakeEmail@
```

這個字串很明顯就不是正常的信箱地址，但是確實可以通過我們上面所設定的斷定規則

為了更精確的判斷，我們可以在 expect() 的客製方法中使用 PHP 的一些方法來進行判斷，如果錯誤就拋出例外 (Exception)

> 請注意每個測試中，至少要有一個斷定

```php
expect()->extend('toBeEmailAddress', function () {
    // 至少放一個斷定在方法中，避免單獨使用此方法導致測試中沒有斷定的錯誤
    expect($this->value)->toBeString();

    if (! filter_var($this->value, FILTER_VALIDATE_EMAIL)) {
        throw new ExpectationFailedException('Email address is not valid');
    }
});
```

## Expect 的簡潔寫法

在剛剛新增聯絡人的例子中，我們用了很多斷定去確認 ORM Model 中的值

```php
expect($contact->first_name)->toBeString(); // pass
expect($contact->first_name)->toBeEmpty(); // fail
expect($contact->first_name)->toBeString()->not->toBeEmpty(); // pass
expect($contact->email)->toEmailAddress(); // pass
expect($contact->phone)->toBeString()->toStartWith('+'); // pass
expect($contact->address)->toBe('No. 23, Fake Rd'); // pass
expect($contact->country)->toBeIn(['us', 'tw']); // pass

```

在 Pest 中我們可以簡化這種寫法

```php
expect($contact)
    ->first_name->toBeString() // pass
    ->last_name->toBeEmpty() // fail
    ->first_name->toBeString()->not->toBeEmpty() // pass
    ->email->toEmailAddress() // pass
    ->phone->toBeString()->toStartWith('+') // pass
    ->address->toBe('No. 23, Fake Rd') // pass
    ->country->toBeIn(['us', 'tw']); // pass
```

Pest 會檢查傳入的物件是否具有該屬性，如果有的話就會生成對應的 Expectation 物件，讓你可以對其使用各種斷定方法

## 參考資料

- [Pest - Faker Plugin](https://pestphp.com/docs/plugins/faker)
- [Pest - Expectations](https://pestphp.com/docs/expectations)
