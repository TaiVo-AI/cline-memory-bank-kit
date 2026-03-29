# Starter Prompts — Bộ Prompt Mẫu

> Sao chép và điều chỉnh các prompt này khi bắt đầu task với Cline.
> Không cần copy file này vào `.clinerules/` — dùng trực tiếp trong chat.

---

## 🚀 Prompt Khởi Động (Dùng đầu mỗi phiên)

```
Tôi cần bạn làm AI pair programmer cho tôi. Trước khi bắt đầu:

1. Đọc các file .clinerules và memory-bank/ để hiểu dự án
2. Khi tôi yêu cầu implement gì, hãy đề xuất kế hoạch trước
3. Luôn chạy test sau khi thay đổi và cho tôi xem kết quả
4. Dùng conventional commits và giải thích lý do

Sẵn sàng chưa? Bắt đầu thôi!
```

---

## 🎯 Project Onboarding — Phân tích dự án mới

> Dùng khi bắt đầu với một codebase chưa quen.

```
Bạn là AI pair programmer mới tham gia dự án này. Hãy:

1. **Phân tích cấu trúc codebase**:
   - Đọc package.json / requirements.txt để biết dependencies
   - Xem cấu trúc thư mục và các entry points chính
   - Kiểm tra tài liệu có sẵn

2. **Hiểu context dự án**:
   - Đây là loại ứng dụng gì?
   - Tech stack chính là gì?
   - Có patterns hoặc conventions nào rõ ràng không?

3. **Xác định trạng thái hiện tại**:
   - Những tính năng nào đã được implement?
   - Có TODO comments hoặc issues nào không?
   - Test coverage như thế nào?

4. **Đề xuất bước tiếp theo**:
   - Bạn sẽ làm gì trước tiên?
   - Có cải tiến nào nên làm ngay không?

Sau khi phân tích xong, hỏi tôi muốn tackle tính năng hoặc vấn đề nào trước.
```

---

## 🐛 Debugging Master — Gỡ lỗi có hệ thống

> Dùng khi gặp bug khó hoặc cần debugging có phương pháp.

```
Tôi đang gặp vấn đề: [MÔ TẢ VẤN ĐỀ]

Hãy debug theo quy trình sau:

1. **Thu thập thông tin**:
   - Đọc kỹ error logs / messages
   - Xem xét các file code liên quan

2. **Đặt giả thuyết**:
   - Liệt kê 3 nguyên nhân có khả năng nhất
   - Giải thích lý do cho mỗi nguyên nhân

3. **Kiểm tra có hệ thống**:
   - Bắt đầu với nguyên nhân có khả năng nhất
   - Test từng giả thuyết với thay đổi tối thiểu
   - Ghi lại cái gì hoạt động và cái gì không

4. **Implement giải pháp**:
   - Fix với error handling đúng cách
   - Thêm test để tránh regression
   - Cập nhật tài liệu nếu cần

Hãy debug từng bước!
```

---

## 🏭 Feature Factory — Implement tính năng mới

> Dùng khi cần implement một tính năng từ đầu, đảm bảo đủ các phase.

```
Tôi muốn implement: [MÔ TẢ TÍNH NĂNG]

Hãy dùng workflow phát triển sau:

## Phase 1: Planning & Architecture

1. **Phân tích tính năng**:
   - Chia nhỏ tính năng thành các sub-components
   - Xác định data models và API endpoints cần thiết
   - Xem xét edge cases và error scenarios

2. **Technical Design**:
   - Chọn patterns và libraries phù hợp
   - Thiết kế database schema changes (nếu có)
   - Plan component hierarchy và state management

## Phase 2: Implementation Strategy

1. **Thứ tự phát triển**:
   - Bắt đầu với data layer (models, API)
   - Build core logic và business rules
   - Tạo UI components và interactions
   - Thêm tests và error handling

2. **Quality Gates**:
   - Type safety và linting checks
   - Unit và integration tests
   - Manual testing scenarios
   - Performance considerations

## Phase 3: Delivery & Documentation

1. **Hoàn thiện**:
   - Code review checklist
   - Cập nhật tài liệu
   - Deployment considerations

Sẵn sàng bắt đầu? Hãy tạo implementation plan chi tiết.
```

---

## 🔍 Code Review Master — Review code chuyên sâu

> Dùng khi cần review PR hoặc đoạn code với tiêu chuẩn senior developer.

```
Hãy review code/PR này với góc nhìn của một senior developer:

## Focus Areas:

### 🏗️ Architecture & Design
- [ ] Có tuân theo patterns của dự án không?
- [ ] Code có được tổ chức và modular không?
- [ ] Có vi phạm design pattern nào không?
- [ ] Solution có phù hợp độ phức tạp không (không over/under-engineer)?

### 🔒 Security & Performance
- [ ] Có security vulnerabilities hoặc data exposure risks không?
- [ ] Performance implications và optimization opportunities?
- [ ] Memory leaks hoặc resource management issues?
- [ ] Error handling và edge cases có đúng không?

### 📝 Code Quality
- [ ] Readability và maintainability?
- [ ] Naming conventions và documentation?
- [ ] Test coverage và quality?
- [ ] Có tuân theo coding standards của dự án không?

### 🔧 Functionality
- [ ] Có đáp ứng requirements không?
- [ ] Có logical errors hoặc bugs không?
- [ ] Xử lý edge cases như thế nào?
- [ ] Integration với existing systems?

## Output Format:
1. **Overall Assessment**: Approve / Request Changes / Comments
2. **Key Strengths**: Những gì được làm tốt
3. **Critical Issues**: Vấn đề phải fix
4. **Suggestions**: Cải tiến nice-to-have
5. **Specific Feedback**: Comments theo từng dòng nếu cần
```
