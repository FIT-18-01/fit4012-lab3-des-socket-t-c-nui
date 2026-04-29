# Peer Review Response

## Thông tin nhóm
- Thành viên 1: Trần Duy Tuấn
- Thành viên 2: Nguyễn Vân Đạt

## Thành viên 1 góp ý cho thành viên 2
- Thành viên 2 đã hoàn thành tốt vai trò vận hành receiver.py và phối hợp nhịp nhàng trong việc thiết lập kết nối Socket. Việc bạn chủ động tạo Branch riêng để thực hiện phần việc cá nhân giúp luồng công việc của nhóm không bị chồng chéo.

## Thành viên 2 góp ý cho thành viên 1
- Sender triển khai đúng luồng mã hóa DES-CBC và xuất log rõ ràng, nhưng phần tài liệu nên nhấn mạnh hơn rủi ro khi gửi key và IV dưới dạng plaintext trên cùng kết nối TCP. Nếu bổ sung thêm mô tả về threat model và giới hạn an toàn của bài lab, phần báo cáo sẽ thuyết phục hơn.

## Nhóm đã sửa gì sau góp ý
- Xử lý bảng mã (Encoding): Nhóm đã bổ sung cấu hình PYTHONIOENCODING='utf-8' và lệnh chcp 65001 vào quy trình vận hành. Việc này giúp khắc phục triệt để lỗi hiển thị tiếng Việt và các ký tự đặc biệt trên Terminal của Windows, đảm bảo log thu được chính xác.

- Tối ưu hóa môi trường Autograding: Sau khi phát hiện lỗi thiếu thư viện trên GitHub Actions, nhóm đã bổ sung file requirements.txt và cấu hình lại .gitignore để đảm bảo hệ thống chấm điểm tự động có thể cài đặt pycryptodome và thực thi mã nguồn thành công.

- Cải thiện Logging: Định dạng lại các bản ghi (logs) theo cấu trúc chuẩn [INFO] kèm theo mốc thời gian và kịch bản mô phỏng, giúp việc theo dõi luồng dữ liệu giữa Sender và Receiver trở nên trực quan và dễ kiểm soát hơn.
