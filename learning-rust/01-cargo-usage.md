# Cargo 筆記

Cargo 為 Rust 為的套件管理工具，可以使用 Cargo 來新建專案與安裝套件

## 相關指令

新建 hello_world 的專案

```bash
cargo new hell_world
```

執行完之後的檔案結構如下

```plaintext
hello_world on  master [?] is 📦 v0.1.0 via 🦀 v1.65.0 on ☁️  (ap-northeast-1) took 6s
➜ tree .
.
├── Cargo.toml
└── src
    └── main.rs

1 directory, 2 files
```

檢查程式碼能否執行

```bash
cargo check
```

執行程式

```bash
cargo run
```

編譯執行檔案

```bash
cargo build
```

如果想要新增套件，可以在 `Cargo.toml` 中的 `[dependencies]` 底下加上想安裝的套件與版本號碼

```toml
[dependencies]
rand = "0.8.5"
```

在執行下方的指令安裝套件

```bash
cargo build
```

更新套件

```bash
cargo update
```
