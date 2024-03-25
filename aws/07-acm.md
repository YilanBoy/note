# AWS Certificate Manager (ACM)

AWS 的憑證管理服務。

## SAA 筆記

- AWS Certificate Manager (ACM) 主要用來管理 SSL/TLS 憑證，憑證可以用在下面這些服務：
  - Elastic Load Balancer (ELB)
  - Cloudfront
  - API Gateway
- ACM 支援自動更新憑證。
- 根據安全考量，ACM 中的憑證無法匯出。
- 你可以自己與 CA 申請憑證，然後匯入 ACM 中，但這樣就不支援自動更新憑證。
