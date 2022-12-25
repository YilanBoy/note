# Lifecycle Hooks

在 PHPUnit 的測試中我們可以使用 `setUp()` 來進行一些前置作業

Pest 提供更靈活的 lifecycle hook 方法，讓我們可以在測試的各個階段進行一些額外的操作

## Lifecycle Hook 方法

在測試中可以使用 `actingAs()` 方法模擬登入用戶的操作

```php
$this->actingAs($this->user)
```

等等，這個 `$this->user` 是怎麼來的 ?

在原本的測試寫法中，可以在建立一個 `protect` 方法 `setUp()` 去設定

```php
protected function setUp(): void
{
    parent::setUp();

    $ths->user = User::factory()->create()
}
```

在 Pest 中你可以使用下面幾個 lifecycle hook 方法，讓你在測試的各個階段，進行一些設定

```php
beforeAll(fn () => var_dump('Before All'));
beforeEach(fn () => var_dump('Before Each'));
afterEach(fn () => var_dump('After Each'));
afterAll(fn () => var_dump('After All'));
```

嘗試在一個 Pest 檔案的測試上方貼上這段程式碼，並執行該檔案的測試

```php
use Inertia\Testing\AssertableInertia as Assert;

beforeAll(fn () => var_dump('Before All'));
beforeEach(fn () => var_dump('Before Each'));
afterEach(fn () => var_dump('After Each'));
afterAll(fn () => var_dump('After All'));

test('can view contents', function () {
    // ...
});

test('can view user information', function () {
    // ...
});
```

你會發現印出來的訊息是

```text
string(10) "Before All"
string(11) "Before Each"
string(10) "After Each"
string(11) "Before Each"
string(10) "After Each"
string(9) "After All"
```

需要注意的是，`beforeAll()` 與 `afterAll()` 沒有辦法使用 Laravel 所提供的功能，因為 Laravel 此時還沒有完成啟動 (boot)

```php
// error: Target class [config] does not exist
beforeAll(fn () => var_dump(config('app.name')));

// correct
beforeEach(fn () => var_dump(config('app.name')));
```

所以剛剛的提問，`$this->user` 是怎麼來的？

我們可以使用 Pest 提供的 lifecycle hook 方法來進行設定

```php
beforeEach(function () {
    $ths->user = User::factory()->create()
});
```

## RefreshDatabase Trait

通常在測試時，我們很常使用 `use RefreshDatabase` 來重置整個資料庫，方便進行測試

在 Pest 中，因為已經沒有 `class` 宣告，我們無法使用 trait 的方式引入 `RefreshDatabase`，所以我們必須換個方式，使用 `uses()`

```php
use Illuminate\Foundation\Testing\RefreshDatabase;

uses(RefreshDatabase::class);

test('can view contents', function () {
    // ...
});
```

但在每個檔案中都寫一段 `uses(RefreshDatabase::class)` 似乎有些囉嗦

你可以考慮在 `tests/TestCase.php` 中加入 `use RefreshDatabase` 這樣就

```php
namespace Tests;

use Illuminate\Foundation\Testing\TestCase as BaseTestCase;
use Illuminate\Foundation\Testing\RefreshDatabase;

abstract class TestCase extends BaseTestCase
{
    use CreatesApplication;

    use RefreshDatabase;
}
```

為了有更好的測試速度，你可以使用 `LazilyRefreshDatabase`，當測試動到資料庫時，才會重置資料庫

```php
namespace Tests;

use Illuminate\Foundation\Testing\TestCase as BaseTestCase;
use Illuminate\Foundation\Testing\LazilyRefreshDatabase;

abstract class TestCase extends BaseTestCase
{
    use CreatesApplication;

    use LazilyRefreshDatabase;
}
```
