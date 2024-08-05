---
layout: default
parent: Rust
nav_order: 5
---

# 註解

註解可以讓你的程式碼更容易理解，**但切勿濫用**，更多時候好的變數與方法名稱就可以取代註解。

在 Rust 中，是使用 `//` 在程式碼中添加註解，被註解的區塊並不會被編譯器給執行。

註解可以可以用來稍微描述複雜程式碼的商業邏輯，讓後續接手開發的人可以更快速的理解。

```rust
// hello, world

fn main() {
    // I'm feeling lucky today
    let lucky_number = 7;
}
```
