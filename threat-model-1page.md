# Lab 8 - Threat model 1 trang

## 1. Tài sản cần bảo vệ

- Nội dung bản tin plaintext.
- DES session key dùng để mã hóa bản tin.
- Khóa riêng RSA của Receiver.
- Tính toàn vẹn của packet truyền qua socket.

## 2. Đối tượng tấn công giả định

Kẻ tấn công có thể nghe lén hoặc sửa đổi dữ liệu trên đường truyền, nhưng không có khóa riêng RSA của Receiver.

## 3. Rủi ro và cơ chế giảm thiểu

| Rủi ro | Cơ chế giảm thiểu trong Lab 8 |
|---|---|
| Nghe lén plaintext | DES-CBC mã hóa nội dung |
| Lộ DES key trên mạng | RSA-OAEP mã hóa DES key bằng public key của Receiver |
| Sửa đổi dữ liệu | SHA-256 giúp phát hiện plaintext sau giải mã không khớp hash ban đầu |
| Gửi packet sai định dạng | Header độ dài 4 byte và hàm parse kiểm tra độ dài |

## 4. Hạn chế còn tồn tại

- DES có kích thước khóa nhỏ, không nên dùng trong hệ thống thật.
- SHA-256 chỉ kiểm tra toàn vẹn sau khi giải mã, chưa thay thế cơ chế xác thực thông điệp chuẩn như HMAC hoặc AEAD.
- Receiver chưa xác thực danh tính Sender.
- Chưa có chống replay attack.
- Chưa có quản lý vòng đời khóa.

## 5. Hướng cải tiến

- Thay DES bằng AES-GCM để có cả mã hóa và xác thực dữ liệu.
- Thêm chữ ký số của Sender để xác thực nguồn gửi.
- Thêm nonce/timestamp để giảm rủi ro replay.
- Không commit private key thật lên GitHub.
