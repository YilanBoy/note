---
layout: default
parent: AWS
nav_order: 22
---

# CloudWatch

CloudWatch 可以用來觀察 AWS 其他服務的狀況，例如 EC2 機器的運行狀態或是 Lambda 函式執行的結果。

## SAA 筆記

- 在 EC2 上 CloudWatch 預設可以看以下這些資訊
  - CPU Utilization
  - Disk Reads activity
  - Network packets out
- 在 EC2 上如果安裝 unified CloudWatch agent，可以看到以下這些客製化的資訊
  - Memory utilization
  - Disk swap utilization
  - Disk space utilization
  - Page file utilization
  - Log collection
