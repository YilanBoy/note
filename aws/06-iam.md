# AWS Identity and Access Management (IAM)

為 AWS 上面管理資源存取權限的全球服務，不分地區。

## SAA 筆記

- IAM User 可以設定 Access Key，可以讓程式使用 Access Key 來獲得操作 AWS 資源的權限。
- 如果你的程式會運行在 EC2 或 ECS 等服務上，建議使用 IAM Role 而不是 Access Key，前者更為安全。
- AWS 帳戶與 IAM User 皆可以使用 MFA (Multi Factor Authentication) 來提升安全性。
- 可以透過 IAM Policy 強制開啟 MFA。
- AWS Security Token Service (STS) 的流程是讓 IAM User 可以透過 Assumes Role 向 IAM Role 取得暫時性的憑證 (Credential)。
- Web Identity Federation 可以讓你藉由其他廠商的 IdP (identity provider) 來取得 AWS 的使用授權 (例如透過 OpenID Connect，簡寫 OIDC)。
- AWS Single Sign-On (SSO) 是使用 STS 做一次性的身份驗證登入多個應用程式 (ex. Slack、Dropbox)。
- AWS IAM Identity Center 提供一個 SSO 入口，讓你可以連結多個服務，背後可以使用微軟 AD (Active Directory)。
