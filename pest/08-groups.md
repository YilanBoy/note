# Group

你可以使用 `group()` 將多個測試放進一個群組中

```php
it('can validate an email', function () {
    $rule = new IsValidEmailAddress();

    expect($rule->passes('email', 'allen@hello.com'))->toBeTrue();
})->group('validation');
```

這樣執行測試就可以使用 `--group` flag 來執行指定群組的測試

> group() 是可以跨檔案的，你可以把位於不同檔案的測試放進同一個群組中

```bash
php artisan test --group=validation
```

如果檔案內全部測試都屬於同一個群組，我們使用下面的語法

```php
uses()->group('validation');
```

如果不想要執行某群組的測試，可以在指令中加上 `--exclude` flag

```bash
php artisan test --exclude=validation
```
