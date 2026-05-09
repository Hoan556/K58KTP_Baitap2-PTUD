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
Sửa settings.py để kết nối MariaDB
Mở file: sudo nano django_app/config/settings.py
tìm đoạn Tìm đoạn này DATABASES = { 'default': { 'ENGINE': 'django.db.backends.sqlite3', 'NAME': BASE_DIR / 'db.sqlite3', } } và thay thành như trong ảnh
<img width="682" height="370" alt="7" src="https://github.com/user-attachments/assets/9d302c5a-f5e8-4d11-8f8d-612300ae6202" />
tạo models.py Mở file: nano django_app/camdo/models.py
<img width="942" height="1007" alt="9" src="https://github.com/user-attachments/assets/ac555cb9-9233-4512-ad74-23e8befe5bf5" />
Sau đó migrate vào MariaDB: docker compose exec django python manage.py migrate
<img width="692" height="508" alt="8" src="https://github.com/user-attachments/assets/b03f34b3-698e-46c0-aa3d-63a7a7f0dab1" />
### Cấu hình Django Admin
Tạo tài khoản admin : docker compose exec django python manage.py createsuperuser
<img width="697" height="292" alt="11" src="https://github.com/user-attachments/assets/0b2bf501-c4ef-44b3-9109-5133d8cc1811" />
-Đăng ký model vào admin
Mở file: nano django_app/camdo/admin.py
<img width="757" height="970" alt="12" src="https://github.com/user-attachments/assets/b84e5b95-0ff6-4a91-ba27-3a38e0e6561b" />
Restart Django: docker compose restart django
Truy cập Django Admin
Mở trình duyệt: http://localhost:8000/admin
Hoặc: http://IP_UBUNTU:8000/admin
- Sau khi login thành công > - KQ được trang admin, yêu cầu đăng nhập, vào trang admin: cho phép thêm sửa xoá dữ liệu các bảng
<img width="1920" height="1080" alt="13" src="https://github.com/user-attachments/assets/81602c68-8596-4c7c-aade-51419d4d932b" />
-Test FK hoạt động đúng
Thêm dữ liệu thử
Thêm khách hàng
<img width="1920" height="1080" alt="14" src="https://github.com/user-attachments/assets/7138966b-45f2-4210-8b2b-eebebe8980f2" />
-Thêm tài sản
<img width="1920" height="1080" alt="16" src="https://github.com/user-attachments/assets/1e55c36b-f069-43dc-add1-3af3a093ad7c" />
<img width="1920" height="1080" alt="17" src="https://github.com/user-attachments/assets/a198b3a2-6eeb-44eb-bdc1-05faf1e41d1b" />
- Thêm hợp đồng cầm
<img width="1920" height="1080" alt="18" src="https://github.com/user-attachments/assets/c998ec2a-c2a8-4d07-97fe-9a4a15547d22" />
Mở website Vào: http://localhost:8000 - > sẽ thấy: Danh sách con nợ chưa trả
<img width="1920" height="1080" alt="29" src="https://github.com/user-attachments/assets/0eabd6b3-47ea-49f8-9f36-0c70f05af01f" />
- Kiểm tra dữ liệu bằng phpMyAdmin
- <img width="1920" height="1080" alt="22" src="https://github.com/user-attachments/assets/45a410fe-c8fd-460d-a8e7-cdd2e7b94592" />
<img width="1920" height="1080" alt="23" src="https://github.com/user-attachments/assets/0674825c-47b7-4922-a41c-97f735330df5" />
<img width="1920" height="1080" alt="24" src="https://github.com/user-attachments/assets/e2b115cd-ce83-468f-afbf-fc77e2526ff1" />
<img width="1920" height="1080" alt="26" src="https://github.com/user-attachments/assets/0dc3f4d8-8fdf-4d8b-839c-7e543ccc21be" />
- Public website bằng Cloudflare Tunnel > - đã có container: cloudflared nên giờ chỉ cần cấu hình tunnel. > - Kiểm tra cloudflared đang chạy: docker logs cloudflared
<img width="697" height="733" alt="30" src="https://github.com/user-attachments/assets/f4fc716b-7f41-4d94-830a-6cb741234119" />
- Lấy domain tunnel tạm thời Chạy: docker exec -it cloudflared sh - Sau đó trong container: cloudflared tunnel --url http://host.docker.internal:8000 - Sửa file: nano django_app/config/settings.py sau khi sửa xong Restart Django container: docker restart django_camdo
<img width="1920" height="1080" alt="34" src="https://github.com/user-attachments/assets/ca834c86-cb5e-4d79-a05a-55c725f73311" />




