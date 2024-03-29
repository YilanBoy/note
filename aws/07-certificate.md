# AWS Certificate Service

AWS 相關的憑證管理服務。

## AWS Certificate Manager (ACM)

- AWS Certificate Manager (ACM) 主要用來管理 SSL/TLS 憑證，憑證可以用在下面這些服務：
  - Elastic Load Balancer (ELB)
  - Cloudfront
  - API Gateway
- ACM 支援自動更新憑證。
- 根據安全考量，ACM 中的憑證無法匯出。
- 你可以自己與 CA 申請憑證，然後匯入 ACM 中，但這樣就不支援自動更新憑證。

## AWS Private CA

AWS Private CA 是一種高度可用的多功能 CA (Certificate Authority)，可協助組織使用私有憑證來保護其應用程式和裝置的安全。
