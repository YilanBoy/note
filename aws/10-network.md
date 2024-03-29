# Network

## Virtual Private Cloud (VPC)

- NAT Gateway 可以在特定 AZ 上建立。為了避免 AZ 掛掉導致 NAT 失效，可以在每個 AZ 的 public subnet 上建立 NAT。
- NACL 預設允許所有進入與出去的流量，要修改流量出入的規則時，建議新增一個 custom NACL rule，而不是修改預設設定。
- NACL 是無狀態的，代表出去與進入的流量都要特別設定。
- 因為 NACL 是無狀態的，因此需要特別設定 ephemeral ports (通常設定在 outbound)。每個作業系統的 port range 不太一樣
  - Linux：32768 - 61000
  - MS：49152 - 65535
- VPC Endpoint 可以讓你透過私有網路與其他 AWS 服務連接 (必須是與 AWS PrivateLink 整合的 AWS 服務)，會透過私有網路的 IP 進行連線。
- Gateway endpoint 是 VPC endpoint 的一種，提供對 S3 與 DynamoDB 的可靠連線 (PrivateLink 不支援 DynamoDB)。
- Gateway endpoint 可以讓多個 subnet 可以共用，較為省錢。
- Gateway endpoint policy 與 S3 bucket policy 都可以來控制訪問 S3 的權限，但後者比較花時間 (你需要對每個 bucket 進行設定)。
- 在 VPC 上建立 Virtual Private Gateway 並設定好 route table，就可以透過 VPN Connection 與地端網路的 Gateway 連接。
- VPC peering 可以做兩個 VPC 的 peering，但注意幾個點：
  - peering 的 CIDR block 不能重複。
  - 假設 A 與 B 做 peering，B 與 C 做 peering，A 與 C 是無法互通的，需要額外再做 peering。
- subnet 有幾點可以記住：
  - subnet 會對應一個 AZ。
  - subnet 建立後會自動對應一個 main route table，能不能對外看有沒有設定 IGW (internet gateway)。
- VPC 中 IPv4 的 CIDR 是一定會有的，而 IPv6 的 CIDR 是可選的 **(2021 年已推出 IPv6 Only Subnet)**。
- VPC 的 IP block 大小由 /16 至 /28。

## Transit Gateway

- AWS Transit Gateway 是一個虛擬路由器，可以將地端或多個 AWS 網路 (**例如 VPC、VPN 或 AWS Direct Connect**) 連接在一起。
- 可以開啟 equal-cost multi-path (ECMP) routing，讓每個 VPN tunnel 都擁有 1.25 Gbps 的頻寬。

![image](https://hackmd.io/_uploads/ryaBAqdEp.png)

## Direct Connect

- Direct Connect 提供了一種專用的、低延遲的直接連接，通過專用電纜連接到 AWS 的網路。
- 連接通常是由 AWS Direct Connect 位置提供商（Direct Connect Location Provider）提供的，不經過公共網際網路。

## Site-to-Site VPN

- Site-to-Site VPN 通常是透過公共網際網路，使用 IPsec 協定，建立安全的點對點連接。這意味著數據在互聯網上進行傳輸。
- 可輸送的數據量較 Direct Connect 少。
