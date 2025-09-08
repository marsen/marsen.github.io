# Pickup Service Mock API

這個資料夾包含了 Pickup Service 的模擬 API 回應資料，用於測試和開發目的。

## 資料夾結構

```
pickup-service/
├── README.md           # 說明文件 (本文件)
└── api/               # API 回應檔案
    ├── done.json      # 完成狀態回應
    ├── shipping.json  # 運送中狀態回應
    ├── fail.json      # 失敗狀態回應
    ├── expiry.json    # 過期狀態回應
    ├── arrived.json   # 已到達狀態回應
    ├── error.json     # 錯誤結果回應
    ├── content-error.json  # 內容錯誤回應
    └── exception.txt  # 異常回應 (純文本)
```

## API 端點說明

### 正常狀態回應

| 檔案 | 狀態 | 說明 | 對應測試案例 |
|------|------|------|-------------|
| `done.json` | DONE | 配送完成 | Case1_Query_Done_waybillNo |
| `shipping.json` | Shipping | 運送中 | Case2_Query_Shipping_waybillNo |
| `fail.json` | FAIL | 配送失敗 | Case3_Query_FAIL_waybillNo |
| `expiry.json` | Expiry | 運單過期 | Case4_Query_Expiry_waybillNo |
| `arrived.json` | Arrived | 已到達取貨點 | Case5_Query_Arrived_waybillNo |

### 異常狀態回應

| 檔案 | 說明 | 對應測試案例 |
|------|------|-------------|
| `error.json` | 結果錯誤 (result: "error") | Case6_Query_Error_Result |
| `content-error.json` | 內容為空 (content: []) | Case7_Query_Error_Content |
| `exception.txt` | 系統異常 (純文本回應) | Case8_Query_Exception |

## 使用方式

### 在 Hexo 中使用

1. 將此 `pickup-service` 資料夾放置於 Hexo 專案的 `source` 目錄下
2. Hexo 會自動將 `api/` 目錄下的檔案作為靜態資源提供服務
3. 可透過以下 URL 存取各種測試回應：

```
http://your-domain.com/pickup-service/api/done.json
http://your-domain.com/pickup-service/api/shipping.json
http://your-domain.com/pickup-service/api/fail.json
http://your-domain.com/pickup-service/api/expiry.json
http://your-domain.com/pickup-service/api/arrived.json
http://your-domain.com/pickup-service/api/error.json
http://your-domain.com/pickup-service/api/content-error.json
http://your-domain.com/pickup-service/api/exception.txt
```

### 在測試中使用

可以直接將這些 URL 用作 mock 服務端點，對應到原本的測試案例：

```csharp
// 原本的測試常數
private const string MockDone = "http://your-domain.com/pickup-service/api/done.json";
private const string MockShipping = "http://your-domain.com/pickup-service/api/shipping.json";
private const string MockFail = "http://your-domain.com/pickup-service/api/fail.json";
private const string MockExpiry = "http://your-domain.com/pickup-service/api/expiry.json";
private const string MockArrived = "http://your-domain.com/pickup-service/api/arrived.json";
private const string MockResultError = "http://your-domain.com/pickup-service/api/error.json";
private const string MockContentError = "http://your-domain.com/pickup-service/api/content-error.json";
private const string MockException = "http://your-domain.com/pickup-service/api/exception.txt";
```

## JSON 回應格式

### 成功回應格式

```json
{
  "result": "success",
  "content": [
    {
      "merchantId": "123",
      "merchantRef": "ABC123",
      "waybillNo": "W123456",
      "locationId": "456",
      "pudoRef": "PUDO789",
      "pudoVerifyCode": "1234",
      "senderId": "Sender123",
      "consigneeId": "Consignee456",
      "customerName": "John Doe",
      "customerAddress1": "123 Main St",
      "customerAddress2": "Apt 456",
      "customerAddress3": "Suburb",
      "customerAddress4": "City",
      "feedbackURL": "https://example.com/feedback",
      "eta": "2023-01-01",
      "codAmt": "50.00",
      "sizeCode": "L",
      "lastStatusId": "DONE",  // 根據不同檔案會有不同的狀態值
      "lastStatusDescription": "Delivered to Customer",
      "lastStatusDate": "2023-01-01",
      "lastStatusTime": "12:34:56",
      "customerMobile": "1234567890",
      "customerEmail": "john.doe@example.com",
      "errorCode": null
    }
  ]
}
```

### 錯誤回應格式

```json
{
  "result": "error",
  "content": []
}
```

## 狀態值對應表

| lastStatusId | 對應 C# enum | 說明 |
|-------------|-------------|------|
| DONE | Status.Finish | 配送完成 |
| Shipping | Status.Processing | 處理中/運送中 |
| FAIL | Status.Abnormal | 異常狀態 |
| Expiry | Status.Abnormal | 異常狀態 |
| Arrived | Status.Arrived | 已到達 |

## 注意事項

- 所有 JSON 檔案都使用 UTF-8 編碼
- `exception.txt` 是純文本檔案，模擬系統異常時的非 JSON 回應
- 測試資料中的個人資料都是範例資料，非真實資料
- 時間格式採用 ISO 日期格式 (YYYY-MM-DD) 和 24 小時制時間格式 (HH:mm:ss)

## 維護說明

如需修改測試資料，請直接編輯對應的 JSON 檔案。修改後重新部署 Hexo 即可生效。

---

*此資料夾是基於 Marsen.NetCore.Dojo 專案中的 PickupService 測試案例建立*