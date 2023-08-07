# Ansible Inventory

Ansible 的 inventory 是一個用來定義主機的清單，可以用來定義主機的 IP 位址、使用者名稱、密碼、SSH 金鑰等等。Ansible 會根據 inventory 的設定來連線主機並且執行指令。

Ansible 是 **agentless** 的，所以不需要在主機上安裝任何 agent，只需要在 Ansible 主機上安裝 Ansible 即可。

## Inventory 的格式

Ansible 的 inventory 有兩種格式，一種是 INI 格式，另一種是 YAML 格式。兩種格式都可以混用，但是不建議這麼做。

### INI 格式

INI 格式的 inventory 類似下面這樣：

```ini
[webservers]
web  ansible_host=foo.example.com ansible_connection=ssh ansible_user=ubuntu ansible_password=ubuntu
web2 ansible_host=bar.example.com ansible_connection=winrm ansible_user=win ansible_password=win

[dbservers]
db1 one.example.com ansible_connection=ssh ansible_user=ubuntu ansible_password=ubuntu
db2 two.example.com ansible_connection=ssh ansible_user=ubuntu ansible_password=ubuntu

localhost ansible_connection=local
```

每個 section 的名稱都是一個 group，每個 group 裡面的每一行都是一個 host。

Inventory 中的每個 host 都可以設定下面的參數：

- ansible_host: 主機的 IP 位址
- ansible_port: SSH 連線的 port，預設為 22
- ansible_user: SSH 連線的使用者名稱，預設為當前使用者
- ansible_ssh_pass: SSH 連線的密碼，**正式環境建議使用 SSH 金鑰登入**
- ansible_ssh_private_key_file: SSH 連線的金鑰檔案路徑
- ansible_connection: 連線的方式，預設為 smart，可以設定為 ssh、local 或是 winrm

### YAML 格式

YAML 格式的 inventory 類似下面這樣：

```yaml
all:
  hosts:
    foo.example.com:
    bar.example.com:
  children:
    webservers:
      hosts:
        foo.example.com:
        bar.example.com:
    dbservers:
      hosts:
        one.example.com:
        two.example.com:
        three.example.com:
```

## 如何使用 inventory

可以建立一個 `inventory.txt` 檔案，並且加入下面的內容：

```ini
[webservers]
web  ansible_host=foo.example.com ansible_connection=ssh ansible_user=ubuntu ansible_password=ubuntu
```

並使用以下的指令測試連線是否成功：

```bash
ansible -i inventory.txt -m ping
```

`-i` 參數用來指定 inventory 的檔案路徑，`-m` 代表要執行的模組，這裡使用的是 `ping` 模組。
