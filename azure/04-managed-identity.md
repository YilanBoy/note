# Azure Managed Identity

當應用程式需要訪問 Azure 資源時，經常需要一組認證，例如使用者名稱和密碼、憑證等等。然而，管理和存儲這些認證可能會帶來風險，例如認證資料外洩、密碼不當管理等等。為了解決這些問題，Azure 提供了 Managed Identity 服務

Azure Managed Identity 可以為應用程式提供一個自動化的管理識別解決方案。Managed Identity 可以讓應用程式在訪問 Azure 資源時，自動以 Azure AD 為基礎建立身份識別

應用程式使用 Managed Identity 可以完全避免在程式碼中明確地存儲認證，從而減少憑證管理上的風險

在 Azure 中，有兩種 Managed Identity

- 系統管理的 managed identity (system assigned managed identity)
- 用戶端管理的 managed identity (user assigned managed identity)

## System Assigned Managed Identity

假設我在 azure 上啟動了一個 A 資源，我可以幫 A 資源開啟 system assigned managed identity，azure 會自動幫我在 azure ad 建立一個身份，並把這個身份與 A 資源綁定在一起

如果我想要讓 B 資源被 A 資源給存取，只需要在 B 服務的權限設定中，將 A 服務的 system assigned managed identity 加入即可

### Example

例如我建立一個 azure function，我想要讓 azure function 獲取能夠 ingest 資料到 azure data explorer database 的權限，那我需要這麼設定

- 在 azure function 頁面上，點選側邊欄的 **Settings -> Identity**，**將 System assigned managed identity 給開啟**
- 到 azure data explorer 的 database 頁面上，點選 **Overview -> Permissions**，點選上方的 **Add**，賦予 **azure function managed identity** Database Admin 的權限

## References

- [Unable to create Azure Data Explorer linked service in Azure Data Factory](https://learn.microsoft.com/en-us/answers/questions/758303/unable-to-create-azure-data-explorer-linked-servic)
