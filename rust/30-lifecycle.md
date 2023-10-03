# 生命週期與引用有效性

Rust 中的每一個引用都有其生命週期（lifetime），也就是引用的有效作用域。

大部分生命週期是可以推斷的，但有時候我們需要使用泛型的生命週期參數來明確指定，確保實際使用時，引用是有效的。

## 生命週期的設計是為了避免懸垂引用

懸垂引用（dangling reference）是指引用了一個已經被釋放的變數的記憶體空間。

Rust 的生命週期設計是為了避免懸垂引用的產生。

```rust
{
    let r;

    {
        let x = 5;
        r = &x;
    }

    // error[E0597]: `x` does not live long enough
    // 這裡會有錯誤， x 已經被釋放，但 r 仍然引用著 x 的記憶體空間
    println!("r: {}", r);
}
```

Rust 編譯器有一個借用檢查器 (borrow checker)，它會確保所有借用都是有效的。

## 函式中的泛型生命週期

讓我們用一個找出最長字串的函式 `longest` 來說明泛型生命週期

```rust
// error[E0106]: missing lifetime specifier
fn longest(x: &str, y: &str) -> &str {

    // Rust 並不知道將要返回的引用是指向 x 或 y
    // 這會導致 Rust 編譯器無法確定返回的引用的生命週期
    if x.len() > y.len() {
        x
    } else {
        y
    }
}

fn main() {
    let string1 = String::from("abcd");
    let string2 = "xyz";

    let result = longest(string1.as_str(), string2);
    println!("The longest string is {}", result);
}
```

上面的程式碼會有錯誤，因為 Rust 編譯器無法確定 `x` 和 `y` 的生命週期，也就無法確定 `result` 的生命週期。

## 引用生命週期的註記

為了解決剛剛提到的問題，我們可以在函式的引數中加入生命週期的註記。

```rust
// 使用 'a 標記生命週期，其中 a 是任意字母
fn longest<'a>(x: &'a str, y: &'a str) -> &'a str {
    if x.len() > y.len() {
        x
    } else {
        y
    }
}
```

這個 `'a` 說明了 `x` 和 `y` 的生命週期是相同的，而且返回值的生命週期也與 `x` 和 `y` 相同。

以下面的例子來看，這是一段可以正常執行的程式碼。

雖然 `string1` 的生命週期在外部作用域，但 `string2` 與 `result` 的生命週期都在內部作用域，因此借用檢查器會認定這是有效的程式碼。

```rust
fn main() {
    let string1 = String::from("long string is long");

    {
        let string2 = String::from("xyz");
        let result = longest(string1.as_str(), string2.as_str());
        println!("The longest string is {}", result);
    }
}
```

來看一個無法通過借用檢查器的例子。

因為 `result` 有可能指向 `string2`，但最後 `string2` 的生命週期卻比 `result` 還要短。

```rust
fn main() {
    let string1 = String::from("long string is long");
    let result;

    {
        let string2 = String::from("xyz");
        result = longest(string1.as_str(), string2.as_str());
    }

    // error[E0597]: `string2` does not live long enough
    println!("The longest string is {}", result);
}
```
