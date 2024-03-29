# API Gateway

- 可以將回傳結果做 cache，減少後面 server 或是 Lambda 的負擔
- API Gateway 可以很輕鬆的做到金絲雀部署 (canary deployment)，金絲雀部署是指先讓一小部分的用戶測試新功能，如果沒問題再逐步擴大至全部用戶
  - 藍綠部署是指同時有新版本與舊版本，並且可以很方面的快速切換，但同一時間只會有一個環境在線上
- API Gateway 可以調整流量限制 (throttling)
- API gateway 只收資料傳輸量的費用，即 pay as you go
- 可以根據 path 將請求送到不同的 backend
- 可以根據 query string 將請求送到不同的 backend
