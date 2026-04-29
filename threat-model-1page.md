# Threat Model - Lab 3

## Thông tin nhóm
- Thành viên 1: Trần Duy Tuấn
- Thành viên 2: Nguyễn Vân Đạt

## Assets
- Nội dung bản tin (Plaintext): Thông tin gốc mà Sender muốn truyền tải tới Receiver một cách bí mật.

- Khóa mã hóa (DES Key): Chìa khóa đối xứng dùng để mã hóa và giải mã dữ liệu.

- Tính toàn vẹn của dữ liệu: Đảm bảo bản tin không bị thay đổi hoặc giả mạo trên đường truyền từ Sender đến Receiver.

## Attacker model
- Eavesdropping (Nghe lén): Có quyền truy cập vào hạ tầng mạng (Sniffing) để bắt các gói tin TCP di chuyển giữa Sender và Receiver.

- Man-in-the-Middle (MITM): Có khả năng can thiệp vào luồng dữ liệu, sửa đổi nội dung gói tin (Tampering) trước khi nó tới tay người nhận.

## Threats
- Lộ thông tin nhạy cảm (Information Disclosure): Do Key và IV được gửi trực tiếp trong Header dưới dạng bản rõ (Plaintext), kẻ tấn công chỉ cần bắt được gói tin là có thể lấy được Key để giải mã toàn bộ bản tin.

- Tấn công sửa đổi dữ liệu (Tampering): Kẻ tấn công có thể thay đổi các byte trong phần Ciphertext. Mặc dù DES-CBC có cơ chế lan truyền lỗi, nhưng nếu không có chữ ký số hoặc MAC, Receiver khó có thể xác định được nội dung đã bị sửa đổi hay chưa (trừ khi lỗi Padding xảy ra).

- Tấn công phát lại (Replay Attack): Kẻ tấn công bắt gói tin hợp lệ và gửi lại nhiều lần cho Receiver, gây nhiễu loạn thông tin hoặc thực hiện các hành vi lặp lại ngoài ý muốn.

## Mitigations
- Tách biệt kênh trao đổi khóa (Out-of-band Key Exchange): Không gửi Key chung với bản mã trong cùng một gói tin. Sử dụng các giao thức như Diffie-Hellman hoặc mã hóa bất đối xứng (RSA) để trao đổi khóa an toàn trước khi truyền tin.

- Sử dụng Message Authentication Code (MAC): Áp dụng thêm HMAC hoặc chữ ký số cho mỗi gói tin để Receiver có thể kiểm tra tính toàn vẹn và xác thực nguồn gốc của bản tin.

- Thêm Timestamp hoặc Nonce: Sử dụng số thứ tự hoặc mốc thời gian trong gói tin để chống lại các cuộc tấn công phát lại (Replay Attack).

- Chế độ mã hóa mạnh hơn: Thay thế DES (vốn đã yếu do độ dài khóa ngắn 56-bit) bằng AES-256 để chống lại các cuộc tấn công vét cạn (Brute-force).

## Residual risks
- Lỗ hổng từ phía người dùng (Endpoint Security): Dù kênh truyền có an toàn, nhưng nếu máy tính của Sender hoặc Receiver bị nhiễm mã độc (Keylogger), kẻ tấn công vẫn có thể lấy được bản rõ hoặc khóa mã hóa trước khi quá trình truyền tin bắt đầu.
- Tấn công từ điển/Vét cạn: Do DES chỉ có 8 byte khóa, với năng lực tính toán hiện nay, kẻ tấn công vẫn có khả năng bẻ khóa nếu thu thập đủ số lượng bản mã cần thiết.