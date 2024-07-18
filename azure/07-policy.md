# Azure Policy

Azure Policy 是可以用來判斷資源是否符合合規性與公司政策的工具。如果資源不符合規定，就可以在一個統一的面板上看到不符合規定的資源，或是乾脆在資源被建立前被阻止。

Azure Policy 可以透過 Management Group 大範圍的監控組織裡的每個 Subscription 底下的資源。幫助公司在雲端上建立一個合規的環境。

## 套用 Policy

你可以在 Azure Policy 頁面底下的 _Authoring -> Assignments -> Assign Policy_ 中套用 Policy。

Policy 可以使用 Azure 預設提供的，也可以自己定義。

## Policy 模板

在 _Authoring -> Definitions -> Policy definition_ 中新增自己的 Policy。

一個簡單的 Policy 如下。當用戶想建立一個機器類型為 `Standard_G` 開頭的虛擬機器，就會被阻止。

```json
{
  "policyRule": {
    "if": {
      "allOf": [
        {
          "field": "type",
          "equals": "Microsoft.Compute/virtualMachines"
        },
        {
          "field": "Microsoft.Compute/virtualMachines/sku.name",
          "like": "Standard_G*"
        }
      ]
    },
    "then": {
      "effect": "deny"
    }
  }
}
```
