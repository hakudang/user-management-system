# DB定義書
 docs/design/db-definition.md  
```md
# DB定義書（Member Table）

## MEMBER（会員）テーブル

| カラム名 | 型 | PK | NN | 説明 |
|----------|----|----|----|------|
| member_id | BIGINT | ○ | ○ | 会員ID |
| name | VARCHAR(255) |  | ○ | 氏名 |
| email | VARCHAR(255) |  | ○ | メールアドレス |
| password | VARCHAR(255) |  | ○ | ハッシュ化PW |
| phone_number | VARCHAR(20) |  | ○ | 電話番号 |
| address | VARCHAR(255) | | | 任意 |
| gender | CHAR(1) | | | M/F |
| date_of_birth | DATE | | | YYYY-MM-DD |
| created_at | DATETIME | | ○ | 作成日時 |
| updated_at | DATETIME | | ○ | 更新日時 |

## Index 設計
- email UNIQUE  
- phone_number INDEX  

## Change History
- 2025-12-04: 初版作成（DANG）
