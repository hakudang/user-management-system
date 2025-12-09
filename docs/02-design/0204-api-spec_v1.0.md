# API仕様書（API Specification）

本書は、User Management System における REST API の仕様を定義する。  
主に以下の機能を対象とする。

- 認証（ログイン／ログアウト／パスワード関連）
- ユーザー管理（Admin/Staff による CRUD）
- 自身のプロフィール管理（End User）
- ロール・権限管理
- 監査ログ閲覧

---

## 1. 共通仕様（Common Specification）

### 1.1 Base URL

```text
/api/v1
```

（例）本番環境: `https://example.com/api/v1`

### 1.2 Request / Response Format

- Request: `application/json`
- Response: `application/json`
- 文字コード: UTF-8

### 1.3 認証方式（Authentication）

- 認証方式: JWT（Bearer Token）
- ログイン成功時に `access_token` を発行
- 以降の認証が必要な API は以下ヘッダーを付与する

```text
Authorization: Bearer <access_token>
```

### 1.4 共通レスポンス形式

#### 成功時

```json
{
  "status": "success",
  "data": {},
  "message": "任意メッセージ（省略可）"
}
```

#### エラー時

```json
{
  "status": "error",
  "message": "エラー概要（ユーザー向け）",
  "errors": {
    "field_name": ["詳細エラー1", "詳細エラー2"]
  }
}
```

- バリデーションエラー: `status = "error"`, HTTP 422
- システムエラー: `status = "error"`, HTTP 500

### 1.5 バリデーション仕様との関係

各 API の入力チェックは、別紙「バリデーション仕様書（validation-spec.md）」に準拠する。  
本書では、主な必須項目やユニーク制約のみを記載する。

### 1.6 ページネーション共通仕様

一覧系 API では以下クエリパラメータを共通利用する。

| パラメータ | 型   | 説明                          | デフォルト |
|-----------|------|-------------------------------|------------|
| page      | int  | ページ番号（1 始まり）        | 1          |
| limit     | int  | 1ページの件数（最大 100 想定） | 20         |

レスポンス例:

```json
{
  "status": "success",
  "data": [...],
  "pagination": {
    "page": 1,
    "limit": 20,
    "total": 52
  }
}
```

---

## 2. API一覧（API List）

### 2.1 認証系（Auth）

| ID  | メソッド | パス                          | 説明                               | 関連UC        |
|-----|----------|-------------------------------|------------------------------------|---------------|
| A01 | POST     | /auth/login                   | ログイン                           | UC-01         |
| A02 | POST     | /auth/logout                  | ログアウト                         | UC-13         |
| A03 | POST     | /auth/password/forgot         | パスワードリセットメール送信      | UC-07         |
| A04 | POST     | /auth/password/reset          | パスワード再設定（トークン経由）  | UC-07         |
| A05 | POST     | /auth/password/change         | ログイン済みユーザーのパスワード変更 | UC-09     |

### 2.2 ユーザー管理（Admin / Staff）

| ID  | メソッド | パス             | 説明                    | 関連UC                  |
|-----|----------|------------------|-------------------------|-------------------------|
| U01 | GET      | /users           | ユーザー一覧取得        | UC-03                   |
| U02 | POST     | /users           | ユーザー新規登録        | UC-02                   |
| U03 | GET      | /users/{id}      | ユーザー詳細取得        | UC-04                   |
| U04 | PUT      | /users/{id}      | ユーザー情報更新        | UC-05                   |
| U05 | DELETE   | /users/{id}      | ユーザー削除（ソフト）  | UC-06                   |
| U06 | PATCH    | /users/{id}/lock | ユーザーロック          | UC-06                   |
| U07 | PATCH    | /users/{id}/unlock | ユーザーロック解除    | UC-06                   |

※ 削除は物理削除ではなく、`status = inactive` などによるソフト削除を想定。

### 2.3 自身のプロフィール（End User）

| ID  | メソッド | パス     | 説明                           | 関連UC   |
|-----|----------|----------|--------------------------------|----------|
| M01 | GET      | /me      | ログインユーザー情報取得      | UC-08    |
| M02 | PUT      | /me      | ログインユーザー情報更新      | UC-08    |

### 2.4 ロール・権限管理

| ID  | メソッド | パス                           | 説明                                 | 関連UC |
|-----|----------|--------------------------------|--------------------------------------|--------|
| R01 | GET      | /roles                        | ロール一覧取得                       | UC-12  |
| R02 | POST     | /roles                        | ロール新規作成                       | UC-12  |
| R03 | PUT      | /roles/{id}                   | ロール名／説明の更新                 | UC-12  |
| R04 | GET      | /permissions                  | 権限一覧取得                         | UC-12  |
| R05 | GET      | /roles/{id}/permissions       | 指定ロールに紐づく権限一覧取得       | UC-12  |
| R06 | PUT      | /roles/{id}/permissions       | 指定ロールの権限設定（上書き）       | UC-12  |

### 2.5 監査ログ / ログイン履歴

| ID  | メソッド | パス                | 説明                          | 関連UC       |
|-----|----------|---------------------|-------------------------------|--------------|
| L01 | GET      | /audit-logs         | 操作ログ & ログイン履歴一覧   | UC-10, UC-11 |

Admin は全体、End User は自分のログイン履歴のみ閲覧可能とする。

---

## 3. API詳細仕様（Endpoint Details）

### 3.1 認証系 API

---

#### A01: ログイン  
**POST** `/auth/login`  
**認証**: 不要  

**用途**  
- Email + Password によるログイン（UC-01）

**Request Body**

```json
{
  "email": "test@example.com",
  "password": "Password123!"
}
```

**バリデーション（概要）**

- `email`: 必須、メール形式
- `password`: 必須  
※ 詳細は validation-spec.md を参照

**Response（成功）**

```json
{
  "status": "success",
  "access_token": "<JWT Token>",
  "data": {
    "user": {
      "id": 1,
      "name": "Taro Yamada",
      "email": "taro@example.com",
      "roles": ["admin"],
      "status": "active"
    }
  }
}
```

**Response（認証失敗）**

HTTP 401

```json
{
  "status": "error",
  "message": "メールアドレスまたはパスワードが違います。"
}
```

---

#### A02: ログアウト  
**POST** `/auth/logout`  
**認証**: 必須  

**用途**  
- ログイン状態の終了（UC-13）

**Header**

```text
Authorization: Bearer <token>
```

**Response**

```json
{
  "status": "success",
  "message": "ログアウトしました。"
}
```

---

#### A03: パスワードリセットメール送信  
**POST** `/auth/password/forgot`  
**認証**: 不要（ログアウト状態を想定）  

**用途**  
- UC-07 パスワードをリセットする（メール送信部分）

**Request Body**

```json
{
  "email": "user@example.com"
}
```

**挙動**

- 該当ユーザーが存在する場合:
  - `password_resets` にレコード登録
  - メールサービス経由で再設定 URL を送信
- 存在しない場合:
  - セキュリティ上の理由により、レスポンスは同じ（存在有無を隠す）

**Response（共通）**

```json
{
  "status": "success",
  "message": "パスワード再設定用のメールを送信しました。"
}
```

---

#### A04: パスワード再設定  
**POST** `/auth/password/reset`  
**認証**: 不要  

**用途**  
- メールのリンクからアクセスして新パスワードを設定（UC-07）

**Request Body**

```json
{
  "token": "<reset token>",
  "password": "NewPass123!",
  "password_confirmation": "NewPass123!"
}
```

**Response（成功）**

```json
{
  "status": "success",
  "message": "パスワードを再設定しました。"
}
```

---

#### A05: パスワード変更（ログイン済み）  
**POST** `/auth/password/change`  
**認証**: 必須（End User）  

**用途**  
- UC-09 パスワードを変更する

**Request Body**

```json
{
  "current_password": "OldPass123!",
  "new_password": "NewPass123!",
  "new_password_confirmation": "NewPass123!"
}
```

**Response（成功）**

```json
{
  "status": "success",
  "message": "パスワードを変更しました。"
}
```

---

### 3.2 ユーザー管理（Admin / Staff）

---

#### U01: ユーザー一覧取得  
**GET** `/users`  
**認証**: 必須（Admin / Staff）  

**用途**  
- UC-03 ユーザー一覧を検索・閲覧する  
- AD-02 / ST-01 画面のバックエンド

**Query Parameters（例）**

| 名称      | 型   | 説明                              |
|-----------|------|-----------------------------------|
| page      | int  | ページ番号                        |
| limit     | int  | 1ページ件数                       |
| keyword   | str  | 氏名／メールアドレスの部分一致    |
| role      | str  | ロールフィルタ（admin/staff/...） |
| status    | str  | active / locked / inactive       |

**Response**

```json
{
  "status": "success",
  "data": [
    {
      "id": 1,
      "name": "Taro Yamada",
      "email": "taro@example.com",
      "phone_number": "000-0000-0000",
      "status": "active",
      "roles": ["admin"]
    }
  ],
  "pagination": {
    "page": 1,
    "limit": 20,
    "total": 52
  }
}
```

---

#### U02: ユーザー新規登録  
**POST** `/users`  
**認証**: 必須（Admin / Staff）  

**用途**  
- UC-02 ユーザーを登録する  
- AD-05 画面のバックエンド

**Request Body**

```json
{
  "name": "Taro",
  "email": "taro@example.com",
  "password": "Password123!",
  "phone_number": "090-1111-2222",
  "address": "Tokyo",
  "gender": "male",
  "date_of_birth": "1990-01-01",
  "roles": ["staff"],
  "status": "active"
}
```

**主なバリデーション**

- `name`: 必須、255文字以内
- `email`: 必須、一意、メール形式
- `password`: 必須、パスワードポリシーに準拠
- `phone_number`: 必須、フォーマット（validation-spec.md）
- `roles`: Admin のみ複数ロール設定可、Staff の場合は制限 など

**Response（成功）**

HTTP 201

```json
{
  "status": "success",
  "data": {
    "id": 10,
    "name": "Taro",
    "email": "taro@example.com"
  }
}
```

**Response（バリデーションエラー）**

HTTP 422

```json
{
  "status": "error",
  "errors": {
    "email": ["既に登録されています。"],
    "password": ["形式が正しくありません。"]
  }
}
```

---

#### U03: ユーザー詳細取得  
**GET** `/users/{id}`  
**認証**: 必須（Admin / Staff）  

**用途**  
- UC-04 ユーザー詳細を閲覧する  
- AD-03 / ST-02 画面のバックエンド  

**Response**

```json
{
  "status": "success",
  "data": {
    "id": 10,
    "name": "Taro",
    "email": "taro@example.com",
    "phone_number": "090-1111-2222",
    "address": "Tokyo",
    "gender": "male",
    "date_of_birth": "1990-01-01",
    "status": "active",
    "roles": ["staff"],
    "created_at": "2025-12-01 10:00:00",
    "updated_at": "2025-12-05 09:00:00"
  }
}
```

---

#### U04: ユーザー更新  
**PUT** `/users/{id}`  
**認証**: 必須（Admin / Staff）  

**用途**  
- UC-05 ユーザー情報を編集する  
- AD-04 / ST-03 画面

**Request Body（例）**

```json
{
  "name": "Taro Updated",
  "phone_number": "090-3333-4444",
  "address": "Osaka",
  "status": "active",
  "roles": ["staff"]
}
```

**Response**

```json
{
  "status": "success",
  "data": {
    "id": 10,
    "name": "Taro Updated"
  }
}
```

---

#### U05: ユーザー削除（ソフト削除）  
**DELETE** `/users/{id}`  
**認証**: 必須（Admin のみ）  

**用途**  
- UC-06 ユーザーをロック / ソフト削除する（削除側）

**挙動（例）**

- `users.status = "inactive"` に更新
- ログイン不可
- 通常の一覧から除外（フィルタ次第）

**Response**

```json
{
  "status": "success",
  "message": "削除しました。"
}
```

---

#### U06: ユーザーロック  
**PATCH** `/users/{id}/lock`  
**認証**: 必須（Admin のみ）  

**用途**  
- UC-06 ロック処理

**Response**

```json
{
  "status": "success",
  "message": "ユーザーをロックしました。"
}
```

---

#### U07: ユーザーロック解除  
**PATCH** `/users/{id}/unlock`  
**認証**: 必須（Admin のみ）  

**Response**

```json
{
  "status": "success",
  "message": "ユーザーのロックを解除しました。"
}
```

---

### 3.3 自身のプロフィール系 API（/me）

---

#### M01: 自分の情報取得  
**GET** `/me`  
**認証**: 必須（End User / Admin / Staff 共通）  

**用途**  
- UC-08 プロフィールを閲覧する  
- EU-02 My Profile 画面

**Response**

```json
{
  "status": "success",
  "data": {
    "id": 5,
    "name": "End User",
    "email": "user@example.com",
    "phone_number": "090-0000-1111",
    "address": "Tokyo",
    "gender": "female",
    "date_of_birth": "1995-01-01",
    "roles": ["user"]
  }
}
```

---

#### M02: 自分の情報更新  
**PUT** `/me`  
**認証**: 必須  

**用途**  
- UC-08 プロフィールを編集する  
- EU-03 My Profile Edit

**Request Body（例）**

```json
{
  "phone_number": "090-9999-8888",
  "address": "Yokohama",
  "gender": "female",
  "date_of_birth": "1995-01-01"
}
```

**Response**

```json
{
  "status": "success",
  "message": "プロフィールを更新しました。"
}
```

---

### 3.4 ロール・権限系 API

---

#### R01: ロール一覧取得  
**GET** `/roles`  
**認証**: 必須（Admin）  

**Response**

```json
{
  "status": "success",
  "data": [
    { "id": 1, "name": "admin", "description": "システム管理者" },
    { "id": 2, "name": "staff", "description": "担当者" },
    { "id": 3, "name": "user", "description": "一般ユーザー" }
  ]
}
```

---

#### R02: ロール新規作成  
**POST** `/roles`  
**認証**: 必須（Admin）  

**Request Body**

```json
{
  "name": "manager",
  "description": "マネージャーロール"
}
```

---

#### R03: ロール更新  
**PUT** `/roles/{id}`  
**認証**: 必須（Admin）  

**Request Body（例）**

```json
{
  "name": "staff",
  "description": "担当者ロール（更新）"
}
```

---

#### R04: 権限一覧取得  
**GET** `/permissions`  
**認証**: 必須（Admin）  

**Response（例）**

```json
{
  "status": "success",
  "data": [
    { "id": 1, "name": "user.view", "description": "ユーザー閲覧" },
    { "id": 2, "name": "user.edit", "description": "ユーザー編集" }
  ]
}
```

---

#### R05: ロールの権限一覧取得  
**GET** `/roles/{id}/permissions`  
**認証**: 必須（Admin）  

**Response（例）**

```json
{
  "status": "success",
  "data": [
    { "id": 1, "name": "user.view" },
    { "id": 2, "name": "user.edit" },
    { "id": 3, "name": "user.delete" }
  ]
}
```

---

#### R06: ロールの権限設定  
**PUT** `/roles/{id}/permissions`  
**認証**: 必須（Admin）  

**Request Body（例）**

```json
{
  "permission_ids": [1, 2, 3]
}
```

**挙動**

- 指定ロールの権限を「この配列で上書き」するイメージ。

---

### 3.5 監査ログ / ログイン履歴 API

---

#### L01: 監査ログ一覧取得  
**GET** `/audit-logs`  
**認証**: 必須  

**用途**

- UC-10 ログイン履歴を閲覧する  
- UC-11 操作ログを閲覧・検索する  
- AD-07 / EU-05 画面のバックエンド

**権限制御**

- Admin: 全ユーザーのログを検索可能  
- End User: 自ユーザーのログのみ取得可能（サーバー側で user_id を制限）  

**Query Parameters（例）**

| 名称      | 型   | 説明                          |
|-----------|------|-------------------------------|
| user_id   | int  | 対象ユーザー（Admin のみ）    |
| action    | str  | LOGIN / UPDATE_PROFILE など   |
| from_date | date | 開始日                        |
| to_date   | date | 終了日                        |
| page      | int  | ページ番号                   |
| limit     | int  | 件数                          |

**Response**

```json
{
  "status": "success",
  "data": [
    {
      "id": 1001,
      "user_id": 1,
      "action": "LOGIN",
      "detail": "{"ip":"127.0.0.1"}",
      "created_at": "2025-12-09 10:00:00"
    }
  ],
  "pagination": {
    "page": 1,
    "limit": 20,
    "total": 200
  }
}
```

---

## 4. ステータスコードポリシー（Status Code Policy）

| HTTPコード | 説明                         | 代表パターン                       |
|-----------|------------------------------|------------------------------------|
| 200       | 成功                         | GET / PUT / PATCH / DELETE 正常   |
| 201       | 新規作成成功                 | POST /users など                   |
| 400       | Bad Request（形式不正）      | 不正な JSON、想定外パラメータなど |
| 401       | Unauthorized（未認証）       | トークンなし／無効トークン         |
| 403       | Forbidden（認可エラー）      | 権限なし                            |
| 404       | Not Found                    | リソース不存在                     |
| 422       | Validation Error             | 入力チェック NG                     |
| 500       | Internal Server Error        | 想定外エラー                        |

---

## 5. モデル定義（参考）

### 5.1 User Resource（レスポンス例）

```json
{
  "id": 1,
  "name": "string",
  "email": "string",
  "phone_number": "string",
  "address": "string",
  "gender": "male/female",
  "date_of_birth": "YYYY-MM-DD",
  "status": "active/locked/inactive",
  "roles": ["admin", "staff"],
  "created_at": "timestamp",
  "updated_at": "timestamp"
}
```

---

## 6. Change History（改版履歴）

| Version | 日付       | 内容                           | 担当 |
|---------|------------|--------------------------------|------|
| 1.0     | 2025-12-09 | 初版 API 一覧・詳細定義        | DANG |
