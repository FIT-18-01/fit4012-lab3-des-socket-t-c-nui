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
1. Tóm tắt kết quả chạy thực tế
- Quá trình truyền nhận diễn ra thông suốt trên môi trường Localhost (cổng 6000).

- Phía Sender: Hệ thống chấp nhận bản tin nhập vào, tự động sinh khóa DES và IV ngẫu nhiên, sau đó đóng gói thành công gói tin nhị phân.

- Phía Receiver: Nhận diện kết nối từ IP 127.0.0.1, bóc tách header chính xác và hoàn trả bản tin gốc không lỗi font.

2. Minh chứng thực tế (Log trích xuất)
- Dữ liệu ghi nhận từ phiên làm việc thực tế của nhóm:

+ Bản tin gốc: hello

+ Key (8-byte): 359f14f5f06b0c20

+ IV (8-byte): af4586f22039f98c

+ Ciphertext (Hex): 04c2ab232f38fcec

+ Trạng thái: Giải mã thành công (Success).

## Kết luận
- Bài học kỹ thuật: Hiểu rõ cách thức hoạt động của TCP Socket và quy trình xử lý dữ liệu ở dạng Byte Stream. Biết cách xử lý lỗi bảng mã (Unicode) trên Terminal Windows để đảm bảo tính sẵn sàng của ứng dụng.
- Bài học bảo mật: Nhận diện được sự nguy hiểm của lỗi "In-band Key Exchange" (truyền khóa cùng dữ liệu). Mặc dù thuật toán DES-CBC bảo vệ được nội dung bản tin, nhưng cách thức thiết kế hệ thống vẫn tồn tại lỗ hổng lớn vì kẻ tấn công có thể dễ dàng lấy được khóa từ Header. Bài lab giúp củng cố tư duy về việc cần có một kênh truyền khóa riêng biệt hoặc sử dụng mã hóa bất đối xứng để trao đổi khóa trong thực tế.
