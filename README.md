# Provider Schema

此 repo 維護 AIS 後端的 **Provider Schema JSON**，描述每個 AI provider 在 model setting 表單中的欄位規格。

後端啟動時會從此 repo 的 raw URL 拉取最新 schema，並每日定時更新（in-memory 快取）。

---

## 檔案

| 檔案 | 說明 |
|------|------|
| `provider_schema.json` | 所有 provider 的表單欄位定義 |

---

## Schema 結構

```json
{
  "providers": [
    {
      "id": "openai",
      "label": "OpenAI",
      "icon": "openai",
      "sections": [
        {
          "id": "basic",
          "label": "Basic Information",
          "collapsible": false,
          "fields": [
            {
              "key": "api_key",
              "label": "API Key",
              "type": "password",
              "required": true,
              "target": "options",
              "placeholder": "sk-..."
            }
          ]
        }
      ]
    }
  ]
}
```

### `type` 欄位說明

| type | 說明 |
|------|------|
| `string` | 一般文字輸入 |
| `password` | 密碼輸入（masked） |
| `url` | URL 輸入 |
| `number` | 數字輸入 |
| `model-info-select` | Model Info 下拉（依 provider 篩選） |
| `tag-picker` | Tag 選擇／建立元件 |
| `cost-limits` | Cost & Limits 複合元件 |
| `available-features` | Available Features 複合元件 |

### `target` 欄位說明

| target | 說明 |
|--------|------|
| `topLevel` | 對應 `GaiModelSetting` 的頂層欄位（`name`、`modelInfoKey` 等） |
| `options` | 存入 `options` JSONB |

---

## 後端設定

在環境變數中設定 raw URL：

```
PROVIDER_SCHEMA_URL=https://your-git-host/provider-schema/raw/main/provider_schema.json
```

未設定時 fallback 到後端 bundled 的 `provider_schema_default.json`。
