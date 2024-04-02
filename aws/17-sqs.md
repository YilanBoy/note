# Simple Queue Service (SQS)

- SQS 是一個全託管的服務，用來解耦服務間的依賴性 (例如一定要等到什麼工作完成才能繼續下一個工作)。
- 如果希望 premium 用戶的任務可以優先處理，建議是開兩個 SQS 佇列，一個給 premium 用戶，另外一個是一般用戶，premium 用戶的任務佇列空了才處理一般用戶的任務佇列。
- SQS Long polling 是指 consumer 可以等 SQS 佇列有訊息後再拉取，此外可以設置最長等多久。
- Long polling 可以有效減少 API 的請求次數。
- 可以設定 `ReceiveMessageWaitTimeSeconds` 來設定 long polling。
- SQS 上面的訊息，可以放置 1-14 天，其中 4 天是預設值。
- 佇列消息數量限制：單個 Amazon SQS 佇列可以包含無限數量的消息。這表示你可以將大量的消息發送到同一個佇列中。
- 對於標準佇列（standard queue），同時處於 "inflight" 狀態的消息數量上限為 120,000 條。
  - 所謂 "inflight" 指的是這些消息已經被接收到並由消費者元件處理，但尚未從佇列中刪除。
- 先進先出類型的佇列（FIFO queue），"inflight" 上限為 20,000 條。
- 在 CloudWatch 上可以監測 `ApproximateAgeOfOldestMessage` 狀態。
