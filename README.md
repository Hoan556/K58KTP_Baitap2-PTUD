#TỔ CHỨC CSDL CHO HỆ THỐNG QUẢN LÝ TIỆM CẦM ĐỒ: viết tay ra giấy, lấy điện thoại chụp lại, upload ảnh lên github (đã nói về các nghiệp vụ trên lớp, ghi bảng)

SỬ DỤNG DOCKER TRÊN UBUNTU ĐỂ:

Mariadb : chứa csdl của hệ thống này
Phpmyadmin: để soi được csdl (chỉ để xem, ko cần tạo bảng từ đây, django sẽ làm hết)
Django: build 1 docker container (dùng Dockerfile): trên nền python, sử dụng django, nhớ mount thư mục để dễ edit, edit dùng: sudo nano ten_file
sau khi có 3 service này trong file docker-compose.yml :

run nó, cấu hình để Django nhận csdl mariadb (sửa file settings.py), cấu hình user login ban đầu, mô tả các bảng trong models.py, .... (đc phép sử dụng AI để làm) => KQ được trang admin, y/c đăng nhập, vào trang admin: cho phép thêm sửa xoá dữ liệu các bảng. các trường là khoá ngoại chỉ việc chọn text (mặc dù là csdl tại trường FK đó lưu ID của PK mà nó tham chiếu : sử dụng phpmyadmin để kiểm chứng)
chú ý kết hợp ssh để chạy lệnh tác động vào django và sudo nano để edit file.
sử dụng template (file html, sử dụng cú pháp jinja2), lấy context từ 1 view home_page, để tạo trang liệt kê các con nợ đến hạn mà chưa trả tiền!
sử dụng cloudflare tunnel để public kết quả lên 1 sub-domain => chụp kết quả
Hướng dẫn:

Tạo thư mục để chứa image tự buid cho django
Vào thư mục đó tạo file tên: Dockerfile (nội dung hỏi AI xem file này cần có nội dung gì, full comment cho từng dòng lệnh trong file này => mục tiêu kép: để hiểu và để hệ thống chạy được)
AI sẽ nói cần thêm file requirements.txt để cài các thư viện cho python (cài qua lệnh pip) => tạo file requirements.txt với nội dung tưng ứng, trong file này cũng comment được => comment xem thư viện nào dùng để làm gì
Sau mỗi lần sửa đỏi có thể phải chạy lệnh dạng : docker compose exec TÊN_SERVICE_DJANGO_CỦA_BẠN python manage.py migrate để tác động vào django (còn nhiều lệnh khác chứ ko luôn như này), để django thay đổi csdl hoặc thay đổi cấu hình.
# Bài làm
## 1. TỔ CHỨC CSDL CHO HỆ THỐNG QUẢN LÝ TIỆM CẦM ĐỒ: viết tay ra giấy, lấy điện thoại chụp lại, upload ảnh lên github (đã nói về các nghiệp vụ trên lớp, ghi bảng)
<img width="1920" height="2560" alt="image" src="https://github.com/user-attachments/assets/f157500e-4fe7-4b62-ad02-ca6ac79ade48" />
— TẠO DOCKERFILE > - Dockerfile dùng để build container Django. Đi vào thư mục django_app : cd django_app Tạo file Dockerfile: sudo nano Dockerfile
<img width="817" height="916" alt="1" src="https://github.com/user-attachments/assets/e3386fdd-8be1-4e57-b0da-f137aaf30d47" />
-TẠO requirements.txt
Tạo file requirements.txt : sudo nano requirements.txt
<img width="951" height="822" alt="2" src="https://github.com/user-attachments/assets/18a55542-b921-42a8-8878-1626cb2ddfa2" />
- TẠO docker-compose.yml
- Quay lại thư mục gốc : cd
Tạo docker-compose.yml: sudo nano docker-compose.yml
<img width="747" height="1015" alt="3" src="https://github.com/user-attachments/assets/181b536a-5180-4d75-aa99-33edfef6bd30" />
-BUILD VÀ CHẠY
Build project: sudo docker compose up -d --build > - Docker sẽ: tải MariaDB tải PhpMyAdmin build Django
<img width="968" height="887" alt="4" src="https://github.com/user-attachments/assets/4aa1a7d2-b00f-4c91-98c7-a4eb3ad60c6e" />
- Tạo app Cầm đồ
Tạo app : sudo docker compose exec django python manage.py startapp camdo
Kiểm tra app đã tạo chưa : ls django_app
<img width="696" height="170" alt="5" src="https://github.com/user-attachments/assets/d9e3141d-e968-4541-b05c-c8c9c6a320fe" />
-Khai báo app camdo
Trong cùng file settings.py
Tìm: INSTALLED_APPS = [ . và Thêm dòng: 'camdo',
<img width="692" height="423" alt="6" src="https://github.com/user-attachments/assets/97b5da52-946f-4bd8-af30-5b697dbb0767" />





