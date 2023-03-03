# netstat 指令

netstat 是一個可以顯示網路連線、網路介面統計數據等網路資訊的命令行工具

## 安裝 netstat

在 Ubuntu 上面安裝 netstat

```bash
apt install net-tools
```

## 使用 netstat 處理 Nginx 無法啟動的 Bug

某次重啟機器之後，發現 nginx 無法啟動

首先用 `tail` 指令查看一下 nginx 的 error log

```bash
# 查看檔案末 100 行的內容
tail -n 100 /var/log/nginx/error.log
```

log 顯示 80 port 已經被使用了，導致 nginx 無法啟動

```plaintext
2023/03/03 03:30:25 [emerg] 2764#2764: bind() to 0.0.0.0:80 failed (98: Unknown error)
2023/03/03 03:30:25 [emerg] 2764#2764: bind() to [::]:80 failed (98: Unknown error)
2023/03/03 03:30:25 [emerg] 2764#2764: still could not bind()
```

用 `netstat` 查看誰正在使用 80 port

```bash
sudo netstat -tulpn
```

以下是各種參數的含義

- `-t`: 顯示 TCP 協議連線狀態
- `-u`: 顯示 UDP 協議連線狀態
- `-l`: 顯示正在聆聽的連線狀態
- `-p`: 顯示與連線相關的進程 ID 和進程名稱
- `-n`: 以數字形式顯示網路地址和連接狀態，而非主機名稱和服務名稱

顯示結果發現 80 port 正在被 apache2 使用中

```plaintext
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name
tcp        0      0 0.0.0.0:80              0.0.0.0:*               LISTEN      2884/apache2: master
tcp6       0      0 :::80                   :::*                    LISTEN      2884/apache2: master
```

我們可以使用 `sudo systemctl list-unit-files` 查看哪些服務預設是被開機啟用的狀態

知道誰是罪魁禍首之後，就把服務給關閉吧

```bash
sudo systemctl stop apache2
sudo systemctl disable apache2
```

重新啟動 nginx，發現 nginx 可以正常工作了

```bash
sudo nginx -t # 檢查 nginx 設定檔案有沒有錯誤
sudo systemctl restart nginx
```
