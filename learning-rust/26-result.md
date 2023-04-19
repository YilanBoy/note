# Result 與可恢復錯誤

如果錯誤沒有嚴重到必須停止程式時，可以使用 `Result`

例如開啟一份檔案，如果檔案不存在，可以嘗試建立檔案

`Result` 列舉有兩個成員，分別是 `Ok` 與 `Err`。而 `<T, E>` 中的 `T` 與 `E` 為泛型

```rust
enum Result<T, E> {
    Ok(T),
    Err(E),
}
```

讓我們嘗試呼叫一個會返回 `Result` 的函式 `File::open()`

```rust
use std::fs::File;

fn main() {
    let f = File::open("hello.txt");
}
```

如果檔案存在則返回 `Ok` 反之不存在則返回 `Err`，如下所例

```rust
use std::fs::File;

fn main() {
    let f = File::open("hello.txt");

    let f = match f {
        Ok(file) => file,
        Err(error) => {
            panic!("There was a problem opening the file: {:?}", error)
        },
    };
}
```

如果檔案不存在顯示的訊息

```bash
thread 'main' panicked at 'There was a problem opening the file: Error { repr:
Os { code: 2, message: "No such file or directory" } }', src/main.rs:9:12
```

## 配對不同的錯誤

在剛才的程式碼中，`File::open` 無論失敗原因是什麼都會觸發 `panic!`。然而，我們實際上可以取得詳細的錯誤原因，並根據不同的錯誤做出不同的處理

```rust
use std::fs::File;
use std::io::ErrorKind;

fn main() {
    let f = File::open("hello.txt");

    let f = match f {
        Ok(file) => file,
        Err(error) => match error.kind() {
            ErrorKind::NotFound => match File::create("hello.txt") {
                Ok(fc) => fc,
                Err(e) => panic!("Tried to create file but there was a problem: {:?}", e),
            },
            other_error => panic!("There was a problem opening the file: {:?}", other_error),
        },
    };
}
```

`File::open` 返回的 `Err`，其值類型為 `io::Error`，他有一個 `kind()` 方法可以取得詳細的錯誤類型，其中 `ErrorKind::NotFound` 代表檔案不存在

如果檔案不存在就使用 `File::create` 建立檔案，但建立檔案也有可能失敗，因此需要 `match` 來處理

仔細看可能會發現 `match` 有點太多，我們可以把上述的程式碼修改成

```rust
use std::fs::File;
use std::io::ErrorKind;

fn main() {
    let f = File::open("hello.txt").map_err(|error| {
        if error.kind() == ErrorKind::NotFound {
            File::create("hello.txt").unwrap_or_else(|error| {
                panic!("Tried to create file but there was a problem: {:?}", error);
            })
        } else {
            panic!("There was a problem opening the file: {:?}", error);
        }
    });
}
```

## 失敗時 panic 的簡寫 `unwrap` 與 `expect`

有時候要處理的錯誤太多，連續使用一堆 `match`，會讓程式碼變的冗長

`Result<T, E>` 定義了很多方法來處理各種情況，其中一種是 `unwrap`，如果是返回 `Ok`，則 `unwrap` 會返回 `Ok` 中的值，反之則呼叫 `panic!`

```rust
use std::fs::File;

fn main() {
    let f = File::open("hello.txt").unwrap();
}
```

如果 `hello.txt` 不存在，`unwrap` 會幫我們呼叫 `panic!`，並提供錯誤訊息

```bash
thread 'main' panicked at 'called `Result::unwrap()` on an `Err` value: Error {
repr: Os { code: 2, message: "No such file or directory" } }',
src/libcore/result.rs:906:4
```

我們可以使用 `expect` 來說明 `unwrap` 返回的錯誤訊息，讓訊息能更加貼近實際的錯誤情形

```rust
use std::fs::File;

fn main() {
    let f = File::open("hello.txt").expect("Failed to open hello.txt");
}
```

顯示的錯誤訊息如下，可以發現統一的錯誤訊息被換成我們在 `expect` 中設定的訊息

```bash
thread 'main' panicked at 'Failed to open hello.txt: Error { repr: Os { code:
2, message: "No such file or directory" } }', src/libcore/result.rs:906:4
```
