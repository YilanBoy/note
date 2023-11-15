# Elastic Load Balancer

最近在準備 SAA 認證，所以就來整理一下 AWS 的 Elastic Load Balancer 相關筆記。

## Elastic Load Balancer 的種類

AWS 提供了四種不同的 Elastic Load Balancer，分別為：

- Application Load Balancer
- Network Load Balancer
- Classic Load Balancer (快退役)
- Gateway Load Balancer

### Application Load Balancer

Application Load Balancer (ALB) 屬於在網路 7 層中屬於第 7 層的應用層，
支援 HTTP/2 與 WebSocket 的協定。

> 考模擬題出現 gRPC 應該使用哪個 LB。因為 gRPC 是運作在 HTTP/2 上，所以是使用 ALB。

ALB 可以將流量導向多個目標機器 (Target Group)。

預設開啟 cross-zone，可以跨 AZ 分配流量。而且不需要為 AZ 間的資料傳輸支付費用。

本身內建 route table，可以根據以下的規則將流量導向不同的目標機器：

- Host-based routing (**one**.example.com, **two**.example.com)
- Path-based routing (example.com/**one**, example.com/**two**)
- Query string or Header-based routing (example.com?user=**one**, example.com?user=**two**)

ALB 有一個固定的 DNS 名稱，但無法綁定一個固定的 IP，如果需要綁定固定 IP，可以考慮使用 Network Load Balancer。

因為機器前面有 ALB 擋著，用戶的真實 IP 會放在 `X-Forwarded-For` 的 HTTP Header 中。

### Network Load Balancer

Network Load Balancer (NLB) 屬於在網路七層中屬於第 4 層的傳輸層，
支援 UDP、TCP 的協定。

相較於 ALB，NLB 擁有較低的延遲與較高的效能，每秒鐘可以處理百萬個請求。

可以綁定一個固定 IP 如果題目敘述需要做 IP 白名單，那大概就是 NLB。

預設不開啟 cross-zone。如果開啟，需要為 AZ 間的資料傳輸支付費用。

### Gateway Load Balancer

Gateway Load Balancer (GWLB) 可以將流量導入到如防火牆、入侵偵測 (IDS) 與預防系統 (IPS)，以及深層封包檢查系統。

它會接聽所有連接埠的全部 IP 封包，並轉送流量至接聽程式規則中指定的目標群組。

Gateway Load Balancer 屬於在網路七層中屬於第 3 層的網路層。

體會在連接埠 6081 上使用 GENEVE 協定交換應用程式流量。
