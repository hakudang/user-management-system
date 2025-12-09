# API仕様書 – 会員登録

## 1. 概要
会員情報を登録するAPI。

## 2. Endpoint


## 3. Request
| 項目 | 型 | 必須 | 説明 |
|------|------|------|------|
| name | string | ○ | 姓名 |
| email | string | ○ | メール |
| password | string | ○ | ハッシュ前 |
| phone_number | string | ○ | 電話 |

## 4. Response
```json
{
  "status": "success",
  "member_id": 123
}

