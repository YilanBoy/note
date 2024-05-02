# AWS Identity and Access Management (IAM)

為 AWS 上面管理資源存取權限的全球服務，不分地區。

## 跨帳號的授予權限

你可以透過 IAM Role 來跨帳號授予權限，例如你可以讓帳號 A 可以讀取帳號 B 的 S3 Bucket。下面就來簡單說明該如何設定。

首先在帳號 B 新增一個讀取 S3 Bucket 的 Policy，並命名為 `delegate_s3_bucket_policy`：

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "s3:ListBucket",
      "Resource": "arn:aws:s3:::my-bucket"
    },
    {
      "Effect": "Allow",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::my-bucket/*"
    }
  ]
}
```

之後一樣在帳號 B 新增一個 Role，並命名為 `delegate_s3_bucket_role`，並且在 Trust Relationship 設定讓該 Role 能被帳號 A 使用：

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "AWS": "arn:aws:iam::A-ACCOUNT-ID:root"
      },
      "Action": "sts:AssumeRole"
    }
  ]
}
```

設定完成之後，將剛剛新增的 Policy 附加到 Role 上。此時帳號 A 就可以透過 AssumeRole API 來取得帳號 B 的 Role，並且使用該 Role 來讀取 S3 Bucket。

```bash
aws sts assume-role --role-arn arn:aws:iam::B-ACCOUNT-ID:role/delegate_s3_bucket_role --role-session-name "my-s3-session"
```

指令的結果如下：

```json
{
    "Credentials": {
        "SecretAccessKey": "wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY",
        "SessionToken": "AQoDYXdzEGcaEXAMPLE2gsYULo+Im5ZEXAMPLEeYjs1M2FUIgIJx9tQqNMBEXAMPLE
CvSRyh0FW7jEXAMPLEW+vE/7s1HRpXviG7b+qYf4nD00EXAMPLEmj4wxS04L/uZEXAMPLECihzFB5lTYLto9dyBgSDy
EXAMPLE9/g7QRUhZp4bqbEXAMPLENwGPyOj59pFA4lNKCIkVgkREXAMPLEjlzxQ7y52gekeVEXAMPLEDiB9ST3Uuysg
sKdEXAMPLE1TVastU1A0SKFEXAMPLEiywCC/Cs8EXAMPLEpZgOs+6hz4AP4KEXAMPLERbASP+4eZScEXAMPLEsnf87e
NhyDHq6ikBQ==",
        "Expiration": "2014-12-11T23:08:07Z",
        "AccessKeyId": "AKIAIOSFODNN7EXAMPLE"
    }
}
```

將上述資訊設成環境變數，就可以使用帳號 B 的 Role 來讀取 S3 Bucket 了：

```bash
export AWS_ACCESS_KEY_ID="AKIAIOSFODNN7EXAMPLE"
export AWS_SECRET_ACCESS_KEY="wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY"
export AWS_SESSION_TOKEN="AQoDYXdzEGcaEXAMPLE2gsYULo+Im5ZEXAMPLEe..."
```

## SAA 筆記

- IAM User 可以設定 Access Key，可以讓程式使用 Access Key 來獲得操作 AWS 資源的權限。
- 如果你的程式會運行在 EC2 或 ECS 等服務上，建議使用 IAM Role 而不是 Access Key，前者更為安全。
- AWS 帳戶與 IAM User 皆可以使用 MFA (Multi Factor Authentication) 來提升安全性。
- 可以透過 IAM Policy 強制開啟 MFA。
- AWS Security Token Service (STS) 的流程是讓 IAM User 可以透過 Assumes Role 向 IAM Role 取得暫時性的憑證 (Credential)。
- Web Identity Federation 可以讓你藉由其他廠商的 IdP (identity provider) 來取得 AWS 的使用授權 (例如透過 OpenID Connect，簡寫 OIDC)。
- AWS Single Sign-On (SSO) 是使用 STS 做一次性的身份驗證登入多個應用程式 (ex. Slack、Dropbox)。
- AWS IAM Identity Center 提供一個 SSO 入口，讓你可以連結多個服務，背後可以使用微軟 AD (Active Directory)。
