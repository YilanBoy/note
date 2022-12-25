# Exception

測試中我們也可以斷定這個測試是否會拋出例外 (Exception)

```php
it('can validate an email', function () {
    // 如果預計這個測試會拋出例外，我們可以使用這個方法來進行斷定
    // 注意這個方法必須放在測試中的最上方
    $this->expectException(InvalidArgumentException::class);

    $rule = new IsValidEmailAddress();

    $rule->passes('email', 123);
});
```

Pest 提供一個更直覺的方法 `throws()`

```php
it('can validate an email', function () {
    $rule = new IsValidEmailAddress();

    $rule->passes('email', 123);
})
->throws(InvalidArgumentException::class);
```

除了斷定丟出哪一種例外，也可以對例外的錯誤訊息進行斷定

```php
it('can validate an email', function () {
    $rule = new IsValidEmailAddress();

    $rule->passes('email', 123);
})
->throws(InvalidArgumentException::class, 'The value must be a string');
```
