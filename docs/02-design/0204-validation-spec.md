# バリデーション仕様
 docs/design/validation-spec.md  
```md
# バリデーション仕様

## 必須項目
- name  
- email  
- password  
- phone_number  

## 文字数
| 項目 | 制限 |
|------|-------|
| name | ≤255 |
| email | ≤255 |
| password | 8〜32 |

## フォーマット
- email → RFC準拠  
- phone → 数字＋ハイフン  
- dob → YYYY-MM-DD  

## Change History
- 2025-12-04: 初版作成（DANG）
