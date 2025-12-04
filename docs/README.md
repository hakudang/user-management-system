# 📘 ドキュメントワークフローガイド（BrSE プロジェクト標準）

プロジェクト全体の「作成 → 管理 → レビュー」フローを統一するためのガイドです。  
ドキュメントは Markdown で作成し、Git で管理し、GitHub に保存します。

---

## 1. 🎯 目的

すべてのドキュメントを **Single Source of Truth（唯一の正本）** として管理する。

- バージョン乱立を防ぎ、GitHub 上の **1 つの正式版** に統一  
- 変更履歴を明確化（who / when / what）  
- Pull Request による透明なレビュー  
- 読みやすく、保守しやすく、共有しやすいドキュメントを実現  

---

## 2. 📁 ドキュメントフォルダ構成

```
docs/
  requirements/
    client-requirements.md        # 顧客要求書
    system-requirements.md        # 要件定義書
    usecase-member-register.md
  design/
    screen-transition.md          # 画面遷移図
    db-definition.md              # DB定義書
    api-spec.md                   # API仕様書
    validation-spec.md            # バリデーション仕様
    business-rules.md             # 業務ルール
    error-spec.md                 # エラー仕様
    mail-spec.md                  # メール仕様
    permission-spec.md            # 権限仕様
  uml/
    erd.mmd                       # ERD（Mermaid）
    seq-member-register.mmd       # シーケンス図
    usecase-member.mmd            # ユースケース図
  templates/
    meeting-minutes-template.md   # 議事録テンプレート
    brse-qa-template.md           # QAテンプレート
    change-log-template.md        # 変更履歴テンプレート
```

---

## 3. 🛠 必要なツール

### VSCode 拡張機能

- Markdown All in One（Markdown編集を高速化）
- Mermaid Preview（UML を直接プレビュー）
- Markdown PDF（PDF 出力）
- GitLens（コミット履歴表示）
- Prettier（Markdown 整形）

### GitHub 機能

- Pull Request  
- Branch Protection（任意）  
- Issue / PR Template  

---

## 4. 🌿 BrSE 標準のドキュメント作成ワークフロー

### 4.1. 新しいブランチを作成する  
`main` を直接編集しないこと。

例：DB定義書を作成する場合

```bash
git checkout -b docs/db-definition
```

---

### 4.2. Markdown ドキュメントを作成・更新する

ルール：

- 1ファイル = 1種のドキュメント  
- 見出しは `##` 以上を使用  
- 要件・パラメータ・ルールは表形式で整理  
- ERD / Sequence 図は Mermaid または Graphviz を使用  

---

### 4.3. 統一フォーマットでコミットする

```bash
feat(docs): add DB definition draft
fix(docs): update validation rules
docs: refine use case flow
```

---

### 4.4. GitHub に Push

```bash
git push origin docs/db-definition
```

---

### 4.5. Pull Request を作成する

PR に記載すべき内容：

- 変更目的  
- 影響範囲  
- UML のスクリーンショット（必要なら）  
- チェックリスト  

例：

```md
### Purpose
- DB定義書（初版）の追加。

### Changes
- docs/design/db-definition.md

### Checklist
- [x] Markdown フォーマット済み
- [x] ERD プレビュー確認
- [x] 用語統一
```

---

## 4.6. レビュー & 承認

- Reviewer（PM / Dev / QA）が行単位でコメント  
- BrSE が回答し、必要に応じて修正  
- PR は小さく分割し、レビューしやすくする  
- 承認後に main へマージ  

---

## 5. 🔄 ドキュメント更新ルール

### 5.1. すべての変更は Pull Request 経由  
main への直接コミットは禁止。

---

### 5.2. 重要ドキュメントは CHANGELOG を持つ  

例：

```md
## Change History
- 2025-12-04: API仕様更新（DANG）
```

---

### 5.3. 「生きているドキュメント（Living Document）」

以下は要求変更とともに必ず更新：

- 要件定義書  
- 画面遷移図  
- バリデーション仕様  

---

## 6. 🔍 ファイル命名ルール

| 種類 | 命名規則 | 例 |
|------|----------|------|
| 要件 | kebab-case | system-requirements.md |
| 設計 | kebab-case | screen-transition.md |
| UML | camelCase / snake_case | erd.mmd / seq-member-register.mmd |
| テンプレ | kebab-case | brse-qa-template.md |

---

## 7. 🧪 マージ前のチェック項目

### 共通チェック

- [ ] Markdown フォーマット  
- [ ] 見出しレベル統一  
- [ ] 長文なら TOC あり  
- [ ] 図が正しく表示される  
- [ ] 誤字脱字なし  
- [ ] 日英越の用語統一  
- [ ] コミットメッセージが明確  

### BrSE 専用チェック

- [ ] 要件に曖昧さがない  
- [ ] UI とフローが一致  
- [ ] DB定義が完全（PK / FK / 型 / 制約）  
- [ ] バリデーション（必須 / 桁数 / 型 / 範囲 / フォーマット）  
- [ ] エラー仕様にコードが明確  
- [ ] 権限仕様がロール別に整合  

---

## 8. 🧭 ドキュメントと開発の同期ポリシー

ロジック変更 → 必ず更新するドキュメント：

1. 要件定義書  
2. 設計書  
3. UML（Sequence / ERD）

開発者は常に `main` の最新ドキュメントに従う。

---

## 9. 📤 クライアント（日本側）へ PDF を提出する場合

- VSCode などで “Markdown PDF: Export (pdf)” を使用  
- PDF は参照用。正本は必ず PR（GitHub）の Markdown  

---

## 10. 🧱 BrSE のドキュメント管理思想

- ドキュメントは Dev が質問せず理解できるレベルに  
- ドキュメントは QA が正確にテストできるレベルに  
- ドキュメントは PM が履歴を追跡できるように  
- ドキュメントは新メンバーが簡単にキャッチアップできるように  
- ドキュメントはプロジェクト拡大に耐えられる構造に  

---
