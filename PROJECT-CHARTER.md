# PROJECT CHARTER

## 1. Thông tin chung

### Tên dự án

**Ứng dụng nhận diện chữ viết tay/OCR tiếng Việt**

### Loại dự án

**Phần mềm ứng dụng**

### Thời gian thực hiện

**17/08/2026 – 29/11/2026**

### Thành viên

| STT | Mã Sinh Viên | Họ và Tên |
|---:|---|---|
| 1 | 247480201036 | Phó Thị Yến Nhi |
| 2 | 247480201032 | Nguyễn Thị Thúy Nga |
| 3 | 247480201045 | Nguyễn Thanh Tiến |
| 4 | 247480201059 | Vũ Quốc Việt |

**Giảng viên phụ trách:** Trần Tuấn Vinh

---

## 2. Mô tả dự án

Dự án **“Ứng dụng nhận diện chữ viết tay và OCR tiếng Việt”** nhằm xây dựng một ứng dụng có khả năng nhận diện nội dung chữ viết tay tiếng Việt từ hình ảnh và chuyển đổi thành văn bản số.

Người dùng có thể cung cấp hình ảnh chứa chữ viết tay thông qua ứng dụng. Hệ thống sẽ thực hiện tiền xử lý hình ảnh, nhận diện nội dung bằng công nghệ OCR, sau đó hiển thị kết quả dưới dạng văn bản. Người dùng có thể kiểm tra kết quả nhận diện.

Dự án tập trung vào việc ứng dụng công nghệ xử lý ảnh và nhận dạng ký tự để hỗ trợ số hóa các tài liệu viết tay tiếng Việt.

---

## 3. Mục đích dự án

Xây dựng một ứng dụng có khả năng tiếp nhận hình ảnh chứa chữ viết tay tiếng Việt, xử lý hình ảnh và nhận diện nội dung thành văn bản số. Kết quả nhận diện có thể được hiển thị, chỉnh sửa và lưu lại để sử dụng.

---

## 4. Mục tiêu

- Xây dựng chức năng tải hoặc nhập ảnh có chữ viết tay.
- Tiền xử lý hình ảnh để nâng cao chất lượng nhận diện.
- Nhận diện chữ viết tay tiếng Việt bằng công nghệ OCR.
- Chuyển nội dung trong ảnh thành văn bản.
- Hiển thị kết quả nhận diện cho người dùng.

---

## 5. Phạm vi dự án

- Nhập ảnh chứa chữ viết tay tiếng Việt.
- Tiền xử lý ảnh.
- Phát hiện và nhận diện ký tự/chữ.
- Xử lý tiếng Việt có dấu.
- Hiển thị văn bản nhận diện.

---

## 6. Sản phẩm đầu ra

- Ứng dụng nhận diện chữ viết tay.
- Cơ sở dữ liệu lưu thông tin cần thiết và kết quả nhận diện.
- Giao diện người dùng.
- Báo cáo phân tích và thiết kế hệ thống.
- Tài liệu hướng dẫn sử dụng và kiểm thử.

---

## 7. Công nghệ dự kiến

- **Ngôn ngữ:** Python
- **Xử lý ảnh:** OpenCV
- **OCR:** Tham khảo mô hình OCR phù hợp với tiếng Việt
- **Giao diện:** Tkinter / PyQt
- **Cơ sở dữ liệu:** SQL Server
- **Môi trường phát triển:** Visual Studio Code

---

## 8. Rủi ro chính

- Chữ viết tay không rõ hoặc quá khác nhau giữa các người viết.
- Ảnh bị mờ, nghiêng, thiếu sáng hoặc có nhiễu.
- Nhận diện sai các ký tự có hình dạng tương tự.
- Khả năng nhận diện tiếng Việt có dấu chưa chính xác.
