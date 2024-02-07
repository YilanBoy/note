# Cargo 筆記

Cargo 為 Rust 為的套件管理工具，可以使用 Cargo 來新建專案與安裝套件。

## 相關指令

使用 cargo 指令新建 hello_world 的專案。

```bash
cargo new hell_world
```

執行完之後的檔案結構如下：

```shell
$ tree .
.
├── Cargo.toml
└── src
    └── main.rs

1 directory, 2 files
```

檢查程式碼能否執行的指令。

```bash
cargo check
```

執行程式的指令。

```bash
cargo run
```

編譯執行檔案的指令。

```bash
cargo build
```

如果想要新增套件，可以在 `Cargo.toml` 中的 `[dependencies]` 底下加上想安裝的套件與版本號碼。

```toml
[dependencies]
rand = "0.8.5"
```

在執行下方的指令安裝套件。

```bash
cargo build
```

但建議還是使用 Cargo 來安裝套件，執行下方的指令安裝套件。

```bash
cargo add <package_name>
```

更新套件的指令。

```bash
cargo update
```

## 全域安裝套件

以 Tauri 為例，安裝 `tauri-cli` 的指令如下：

```bash
cargo install tauri-cli
```

安裝的套件預設會放在 `$HOME/.cargo/bin` 底下 (MacOS)。

之後就可以使用 `tauri` 指令來建立專案。

```bash
cargo tauri init
```

## 執行測試

可以使用 `cargo test` 來執行測試。 測試執行會產生一個測試執行檔案。

```bash
cargo test
```

## 發佈設定檔 (release profiles)

Cargo 有兩個主要的設定檔：

- `dev` 設定檔會在當你對 Cargo 執行 cargo build 時所使用。
- `release` 設定檔會在當你對 Cargo 執行 cargo build --release 時所使用。

dev 設定檔預設定義為適用於開發時使用，而 release 設定檔預設定義為適用於發佈時使用。

```toml
# opt-level 設定控制了 Rust 對程式碼進行優化的程度，範圍從 0 到 3
# 0: 不進行優化
# 3: 進行最大優化，但會增加編譯時間
[profile.dev]
opt-level = 0

[profile.release]
opt-level = 3
```

對於完整的設置選項與每個設定檔的預設列表，請查閱 [Cargo 的技術文件](https://doc.rust-lang.org/cargo/reference/profiles.html)。
