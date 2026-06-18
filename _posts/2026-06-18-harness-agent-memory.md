# I. Agent Memory: How Agents Remember and Forget
## (Cách các Agent ghi nhớ và lãng quên)

Bài nói chuyện giải thích về cấu trúc bộ nhớ của một trợ lý AI (AI Agent).

Bản chất của mô hình ngôn ngữ lớn (LLM) là **không tự thay đổi trọng số** hay **tự ghi nhớ** sau khi cửa sổ ngữ cảnh (context window) kết thúc. Vì vậy, để Agent có thể "nhớ", lập trình viên cần xây dựng một **hệ thống kiến trúc bộ nhớ** bao bọc xung quanh mô hình (gọi là *harness primitives*).

---

# 1. Bốn Loại Bộ Nhớ Của AI Agent

Bộ nhớ của Agent được chia thành 4 loại tương tự như trong tâm lý học nhận thức của con người.

## 1.1. Working Memory (Bộ nhớ làm việc)

Đây chính là **Context Window** hiện tại.

Có thể hình dung như một **chiếc bàn làm việc**, nơi chứa:

- Các tin nhắn chat gần đây
- Chỉ thị hiện tại
- Kết quả từ các công cụ (tools)

### Ưu điểm
- Cho phép Agent tập trung vào nhiệm vụ đang xử lý.

### Hạn chế
- Mất hoàn toàn khi kết thúc phiên làm việc.
- Dễ bị quá tải nếu đưa quá nhiều tài liệu vào cùng lúc.

---

## 1.2. Episodic Memory (Bộ nhớ sự kiện)

Lưu trữ lịch sử các sự kiện đã xảy ra gắn với thời điểm cụ thể.

Ví dụ:

- Phiên sửa lỗi vào thứ Sáu tuần trước
- Lịch sử xử lý một GitHub Issue
- Các lần triển khai hệ thống

### Vai trò
Giúp Agent tìm lại những "lát cắt" trong quá khứ thay vì chỉ truy xuất các sự thật rời rạc.

---

## 1.3. Semantic Memory (Bộ nhớ ngữ nghĩa)

Lưu trữ các kiến thức và sự thật ổn định mang tính dài hạn.

Ví dụ:

- Quy định cấu trúc thư mục của dự án
- Thông tin tài khoản người dùng
- Quy tắc nghiệp vụ (business rules)

### Thách thức
Các thông tin này có thể:

- Trở nên lỗi thời
- Không còn chính xác
- Cần được cập nhật định kỳ

---

## 1.4. Procedural Memory (Bộ nhớ thủ tục)

Lưu trữ **cách thực hiện công việc (How-To)**.

Bao gồm:

- Runbooks
- Checklists
- Workflow
- Quy trình tự động hóa

Trong hệ thống Agent, loại bộ nhớ này thường xuất hiện dưới dạng:

- Tool Schemas
- Skill Files
- Orchestration Code

---

# 2. Mô Hình Kiến Trúc Bộ Nhớ (Memory Architecture)

Một phép ẩn dụ dễ hiểu:

> Các kho lưu trữ dài hạn giống như một tủ đựng tài liệu, còn Working Memory chính là mặt bàn làm việc. Mô hình AI chỉ có thể xử lý những gì được đặt trên mặt bàn.

## Kiến trúc gồm 3 thành phần chính

### 2.1. Durable Stores (Kho lưu trữ dài hạn)

Lưu trữ:

- Lịch sử chat
- Tài liệu
- Quy trình
- Skill files
- Tri thức dài hạn

---

### 2.2. Context Builder (Bộ dựng ngữ cảnh)

Đây là thành phần quan trọng nhất.

Nhiệm vụ:

1. Tìm kiếm thông tin
2. Xếp hạng mức độ liên quan
3. Chọn lọc nội dung quan trọng
4. Đưa vào Context Window

Có thể xem đây là người quản lý việc lấy tài liệu từ "tủ hồ sơ" và đặt lên "bàn làm việc".

---

### 2.3. Model (Mô hình AI)

Tiếp nhận dữ liệu đã được chuẩn bị trong Context Window để:

- Suy luận
- Ra quyết định
- Thực hiện hành động tiếp theo

---

# 3. Tầm Quan Trọng Của Việc "Lãng Quên" (Forgetting)

Một hệ thống ghi nhớ mọi thứ sẽ nhanh chóng:

- Quá tải
- Tốn chi phí
- Khó tìm được thông tin quan trọng

Do đó, khả năng "quên" là một hình thức **vệ sinh bộ nhớ** cần thiết.

## Các chiến lược lãng quên

### 3.1. Temporal Decay (Suy giảm theo thời gian)

- Các ký ức cũ sẽ dần bị giảm độ ưu tiên.
- Thông tin mới được ưu tiên hơn.

---

### 3.2. Contradiction Handling (Xử lý mâu thuẫn)

Khi xuất hiện thông tin mới xung đột với dữ liệu cũ:

- Xác minh tính đúng đắn
- Cập nhật sự thật mới
- Loại bỏ hoặc giảm ưu tiên dữ liệu cũ

---

### 3.3. Compression (Nén dữ liệu)

Chuyển:

- Các cuộc hội thoại dài
- Nhật ký chi tiết

thành:

- Tóm tắt ngắn gọn
- Các insight quan trọng

Mục tiêu là giảm dung lượng nhưng vẫn giữ được ý nghĩa cốt lõi.

---

# 4. Năm Câu Hỏi Để Đánh Giá Hệ Thống Bộ Nhớ Của Agent

Khi xây dựng hoặc đánh giá một AI Agent, hãy tự hỏi:

1. **Bộ nhớ làm việc hiện tại đang chứa những gì?**

2. **Những phiên làm việc (sessions) nào trong quá khứ thực sự quan trọng?**

3. **Những sự thật hoặc thông tin nào hiện đang là mới nhất?**

4. **Quy trình làm việc (workflow) nào đang được áp dụng?**

5. **Những gì cần được lãng quên hoặc giảm mức ưu tiên?**

---

# Kết Luận

Một AI Agent không thực sự "nhớ" theo cách con người ghi nhớ. Thay vào đó, khả năng nhớ của Agent đến từ một **kiến trúc bộ nhớ bên ngoài LLM** bao gồm:

- **Working Memory** → những gì đang được xử lý
- **Episodic Memory** → những gì đã xảy ra
- **Semantic Memory** → những gì được biết
- **Procedural Memory** → cách thực hiện công việc

Hiệu quả của Agent phụ thuộc rất lớn vào khả năng:

- Truy xuất đúng thông tin
- Cập nhật kiến thức mới
- Và quan trọng không kém: **biết quên những gì không còn cần thiết**

https://www.youtube.com/watch?v=PxuMqeIqCEo&t=64s
