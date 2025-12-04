# 📘 Document Workflow Guide (BrSE Project Documentation)

Hướng dẫn quy trình tạo – quản lý – review tài liệu của toàn dự án.  
Tài liệu được viết bằng Markdown, quản lý bằng Git, lưu trữ trên GitHub.

---

## 1. 🎯 Mục tiêu

Giữ toàn bộ tài liệu dự án ở dạng **single source of truth**:

- Không còn nhiều phiên bản → chỉ **1 bản chính thức** trên GitHub  
- Lịch sử thay đổi rõ ràng (who/when/what)  
- Review tài liệu minh bạch qua Pull Request  
- Tài liệu dễ đọc, dễ bảo trì, dễ chia sẻ  

---

## 2. 📁 Cấu trúc thư mục tài liệu

```
docs/
  requirements/
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
    erd.mmd                       # ERD (Mermaid)
    seq-member-register.mmd       # Sequence Diagram
    usecase-member.mmd            # Use Case Diagram
  templates/
    meeting-minutes-template.md   # 議事録テンプレ
    brse-qa-template.md           # QAテンプレ
    change-log-template.md        # 変更履歴テンプレ
```

---

## 3. 🛠 Công cụ cần có

### VSCode extensions

- Markdown All in One – soạn thảo Markdown nhanh  
- Mermaid Preview – xem UML trực tiếp  
- Markdown PDF – xuất bản PDF  
- GitLens – xem lịch sử commit  
- Prettier – format tài liệu  

### GitHub features

- Pull Request  
- Branch Protection (option)  
- Issue Template + PR Template  

---

## 4. 🌿 Workflow viết tài liệu chuẩn BrSE

### 4.1. Tạo nhánh mới cho tài liệu  
Không edit trực tiếp trên `main`.

Ví dụ tạo tài liệu DB Definition:

```
git checkout -b docs/db-definition
```

---

### 4.2. Tạo hoặc cập nhật tài liệu Markdown

Quy tắc:

- Mỗi file đại diện đúng 1 loại tài liệu  
- Dùng heading cấp 2 trở lên (`##`, `###`)  
- Dùng bảng để mô tả yêu cầu, tham số, rule  
- UML (ERD, sequence...) dùng Mermaid hoặc Graphviz  

---

### 4.3. Commit với format thống nhất

```
feat(docs): add DB definition draft
fix(docs): update validation rules
docs: refine use case flow
```

---

### 4.4. Push lên GitHub

```
git push origin docs/db-definition
```

---

### 4.5. Tạo Pull Request

Nội dung cần ghi trong PR:

- Mục đích sửa đổi  
- Phạm vi tài liệu ảnh hưởng  
- Screenshot (nếu có UML)  
- Checklist hoàn thành  

Ví dụ:

```md
### Purpose
- Thêm DB定義書 bản đầu tiên.

### Changes
- docs/design/db-definition.md

### Checklist
- [x] Format chuẩn Markdown
- [x] ERD preview OK
- [x] Thuật ngữ thống nhất
```

---

### 4.6. Review & Approve

- Reviewer (PM/Dev/QA) comment vào từng dòng  
- BrSE trả lời rõ ràng, sửa nếu cần  
- Cố gắng giữ PR nhỏ để review dễ  
- Khi được approve → merge vào main  

---

## 5. 🔄 Quy tắc cập nhật tài liệu

### 5.1. Mọi thay đổi phải đi qua Pull Request  
Không được commit trực tiếp lên `main`.

---

### 5.2. Tài liệu quan trọng phải có CHANGELOG  
Ví dụ:

```
## Change History
- 2025-12-04: Updated API spec (DANG)
```

---

### 5.3. Tài liệu dạng “sống” (living documents)

- 要件定義  
- 画面遷移図  
- バリデーション仕様  

→ Khi thay đổi requirement phải cập nhật ngay.

---

## 6. 🔍 Quy tắc đặt tên file

| Loại tài liệu | Quy tắc | Ví dụ |
|---------------|---------|--------|
| Requirements | kebab-case | system-requirements.md |
| Design | kebab-case | screen-transition.md |
| UML | camelCase hoặc snake_case | erd.mmd, seq-member-register.mmd |
| Template | kebab-case | brse-qa-template.md |

---

## 7. 🧪 Quy tắc kiểm tra tài liệu trước khi merge

### Checklist chung:

- [ ] Format Markdown chuẩn  
- [ ] Heading đồng nhất  
- [ ] TOC tự động (nếu dài)  
- [ ] Diagram hiển thị OK  
- [ ] Không lỗi chính tả  
- [ ] Dùng đúng thuật ngữ Nhật–Anh–Việt  
- [ ] Commit rõ ràng  

### Checklist riêng cho BrSE:

- [ ] Requirement → không ambiguous  
- [ ] Flow logic đúng và khớp UI  
- [ ] DB định nghĩa đủ khóa, kiểu dữ liệu, rule  
- [ ] Validation đầy đủ (必須 / 文字数 / 型 / 範囲 / フォーマット)  
- [ ] Error Spec có mã error rõ ràng  
- [ ] Permission Spec phân quyền đúng role  

---

## 8. 🧭 Policy đồng bộ tài liệu với Dev

Thay đổi logic → cập nhật 3 chỗ:

1. 要件定義書 (Requirement)  
2. 設計書 (Design)  
3. UML (Sequence / ERD)

→ Dev luôn làm theo tài liệu mới nhất trên `main`.

---

## 9. 📤 Xuất tài liệu cho khách Nhật

Nếu cần gửi PDF:

- Right-click file → “Markdown PDF: Export (pdf)”  
- Upload file vào Google Drive hoặc gửi mail kèm link  
- PR luôn là bản chính; PDF chỉ để tham khảo  

---

## 10. 🧱 Tư duy quản lý tài liệu của BrSE

- Tài liệu phải rõ ràng để Dev không hỏi lại  
- Tài liệu phải thống nhất để QA test đúng  
- Tài liệu phải có lịch sử để PM trace được  
- Tài liệu phải cấu trúc chuẩn để onboarding nhanh  
- Tài liệu phải mở rộng được khi dự án lớn lên  

