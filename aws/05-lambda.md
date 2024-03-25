# Lambda

為 AWS 無伺服器運算服務，優點是根據用量付費，沒有最低消費，完全不使用就不會有費用產生
，但缺點就是沒有上限，用量大起來費用會十分驚人。

## Function

放在 Lambda 上面的程式稱為 Function。Lambda 背後的技術是使用 AWS 自己研發的 [Firecracker](https://firecracker-microvm.github.io/)，可以快速地開啟一個 MicroVM。

## Layer

Lambda 無法讓用戶 SSH 至伺服器上設定環境。
如果你有想安裝的程式碼依賴套件，可以將依賴套件的相關檔案打包成 zip 並上傳到 Lambda Layer。

Layer 可以給多個 Function 重復使用，當 Function 會解壓縮 Layer 中的檔案並放在 `/opt` 資料夾底下。

可以參考[這篇文章](https://community.aws/content/2d6gQDnHqIbWLLKikuSfwmOZrym/step-by-step-guide-to-creating-an-aws-lambda-function-layer)學習如何打包 Python 的依賴套件檔案。

**注意每個 Function 至多只能使用 5 個 Layer**。

## SAA 筆記

- **Lambda 執行時間最多為 15 分鐘**，超過這個時間的任務就不適合使用 Lambda。
- Lambda 可以設置環境變數來讓 Function 使用。可以直接在 Portal 上看到，如果有安全上的疑慮可以使用冷啟動，
  即在程式碼啟動後去 Parameter Store 或是 Secret Manager 中取得需要的設定值，不使用環境變數。
  缺點是第一次啟動會較慢。
- Lambda 的環境變數可以使用 KMS (Key Management Service) 的金鑰來加密。
- Lambda Function 可以建立一個 URL 來讓外部用戶訪問或是呼叫。
  如果想使用自己的 FQDN，可以搭配 API Gateway 做 Custom Domain Name 的 Mapping。
- 提供 10GB 暫時性的存儲空間，詳細可以參考[這裡](https://aws.amazon.com/tw/blogs/aws/aws-lambda-now-supports-up-to-10-gb-ephemeral-storage/)。
