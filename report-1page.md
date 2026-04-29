# Report 1 page - Lab 3

## Thông tin nhóm
- Thành viên 1: Trần Duy Tuấn
- Thành viên 2: Nguyễn Vân Đạt

## Mục tiêu
Bài Lab 3 tập trung vào việc xây dựng hệ thống truyền nhận dữ liệu an toàn thông qua TCP Socket kết hợp thuật toán mã hóa đối xứng DES trong chế độ CBC. Mục tiêu chính là giúp sinh viên hiểu rõ quy trình đóng gói dữ liệu (Framing), vai trò của Vector khởi tạo (IV) và cơ chế Padding PKCS#7 để đảm bảo tính toàn vẹn của khối dữ liệu 8-byte. Qua đó, sinh viên có thể đánh giá các rủi ro bảo mật thực tế khi truyền khóa trên cùng một kênh dữ liệu.

## Phân công thực hiện
- Trần Duy Tuấn:
+ Quản lý Sender: Đảm nhiệm vận hành tiến trình gửi tin, kiểm tra tính đúng đắn của Key/IV được sinh ra.

+ Phân tích rủi ro: Chịu trách nhiệm chính cho file threat-model-1page.md, phân tích chuyên sâu về lỗ hổng "In-band Key Exchange" trong code mẫu.

+ Chủ trì báo cáo: Tổng hợp kết quả và hoàn thiện file report-1page.md.

- Nguyễn Vân Đạt:
+ Quản lý Receiver: Đảm nhiệm vận hành tiến trình nhận tin, theo dõi luồng giải mã.

+ Thực thi kiểm thử & Logs: Chạy các bộ test case mẫu, trích xuất log từ màn hình Terminal vào thư mục logs/.

+ Peer Review: Hoàn thiện file peer-review-response.md.

## Cách làm
- Triển khai Sender: Sử dụng tiến trình sender.py để khởi tạo kết nối TCP đến địa chỉ localhost:6001. Chương trình tự động sinh khóa DES (8 byte) và IV (8 byte), thực hiện mã hóa bản rõ nhập từ bàn phím và đóng gói thành một Packet hoàn chỉnh (Header + Ciphertext).

- Triển khai Receiver: Chạy receiver.py ở chế độ lắng nghe (Listening). Khi có kết nối, chương trình sẽ đọc 20 byte đầu tiên để bóc tách thông tin Header, sau đó dùng chính Key và IV nhận được để giải mã phần dữ liệu còn lại.

- Mã hóa DES-CBC: Sử dụng thư viện pycryptodome với chế độ CBC. Dữ liệu được đưa qua hàm pad() chuẩn PKCS#7 để đảm bảo kích thước luôn là bội số của 8 byte trước khi mã hóa.

- Kiểm thử thủ công: Chạy song song 2 Terminal trên Windows, cấu hình chcp 65001 để hiển thị tiếng Việt, thực hiện gửi nhận bản tin thực tế.

## Kết quả
TODO_STUDENT: Tóm tắt kết quả chạy, ảnh/log minh chứng, và ca kiểm thử quan trọng.

## Kết luận
TODO_STUDENT: Rút ra bài học kỹ thuật và bài học bảo mật từ bài lab.
