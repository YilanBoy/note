# Generic、Traits 與 Lifetimes

如果你有一段程式碼可以接收不同種類型的值，並使用相同的邏輯處理，那麼就可以考慮使用泛型。
泛型並不會給定一個固定類型，而是呼叫時在指定一個類型。
Rust 本身已經有內建一些泛型讓我們直接使用，例如 `Option<T>`、`Vce<T>`、`HashMap<K, V>` 與 `Result<T>`

Traits 可以用來定義泛型行為的方法，traits 可以將泛型限制為擁有特定行為的類型，而非任意類型

Lifetimes 允許我們向編譯器提供引用如何相互關聯的泛型。
Rust 的生命週期功能允許在很多場景下借用值的同時，仍然使編輯器能夠檢查這些引用的有效性

## 提取函式來減少重複

假設我們有一個尋找最大值的函式

```rust
fn main() {
    // 將列表存放在變數 number_list 中
    let number_list = vec![34, 50, 25, 100, 65];

    // 將列表的第一項存放在可變變數 largest 中
    let mut largest = &number_list[0];

    // 將 largest 與 number_list 中所有元素一一比對
    // 如果元素比 largest 大，就將元素的值放入 largest 中取代掉原本的值
    for number in &number_list {
        if number > largest {
            largest = number;
        }
    }

    println!("The largest number is {}", largest);
}
```

如果們需要多次找尋列表的最大值時，可以將上面的程式碼抽取出來，變成一個獨立的函式

```rust
fn largest(list: &[i32]) -> &i32 {
    let mut largest = &list[0];

    for item in list {
        if item > largest {
            largest = item;
        }
    }

    largest
}

fn main() {
    let number_list = vec![34, 50, 25, 100, 65];

    let result = largest(&number_list);
    println!("The largest number is {}", result);

    let number_list = vec![102, 34, 6000, 89, 54, 2, 43, 8];

    let result = largest(&number_list);
    println!("The largest number is {}", result);
}
```

上述的修改是我們在重構程式碼時很常會遇到的狀況

1. 找出重複程式碼
2. 將重複的程式碼抽取出來，變成一個獨立的函式，並設定好參數與返回值
3. 將重複程式碼的地方修改為呼叫函式

下一步，我們將利用泛型來執行這些相同的步驟，以減少程式碼重複的情況。
就如同函式的內容可以操作抽象的列表，而非已經固定為數字的列表 (如上述的例子)，泛型可以讓程式碼對抽象型態進行操作。

假設我們有兩個函式，一個可以如同上面的範例，找出數字列表中的最大值。
而另外一個則是可以找出字元 (char) 列表中的最大值。
這兩個函式的邏輯完全相同，如果想要避免重複撰寫一樣的邏輯，我們可以在這裡考慮使用泛型
