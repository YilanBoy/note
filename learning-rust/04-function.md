# 函數

- 函數需要在前面加上 `fn` 關鍵字
- Rust 中函數名稱與變數名稱使用 snake_case 規範
- Rust 程式碼的入口點是 `main` 函數
- 函數的順序並不重要，只要一宣告好，你可以在其前或其後進行呼叫

```rust
fn main() {
    println!("Hello, world!");

    another_function();
}

fn another_function() {
    println!("Another function.");
}
```
