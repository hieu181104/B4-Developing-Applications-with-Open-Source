# KHAI THÁC N8N ĐỂ TỰ ĐỘNG ĐĂNG BÀI LÊN WORDPRESS 
- Môn học: Phát triển ứng dụng với mã nguồn mở - TEE0421
- Họ và tên: Nguyễn Trung Hiếu
- Công nghệ sử dụng: Wordpress, Docker, MariaDB, PhpMyAdmin, Cloudflare, N8N

---

## 📌 MỤC LỤC
[1. Giới thiệu](#1-giới-thiệu)

[2. Các bước thực hiện](#2-các-bước-thực-hiện)

[3. Kết quả đạt được](#3-kết-quả-đạt-được)

[4. Nhận xét](#4-nhận-xét)

---

## 1. GIỚI THIỆU

Bài tập này mở rộng từ hệ thống WordPress đã được triển khai từ bài tập trước bằng Docker Compose. Bổ sung thêm service n8n để xây dựng hệ thống tự động đăng bài lên WordPress thông qua Telegram Bot và Google Gemini AI.

Người dùng chỉ cần gửi nội dung qua Telegram Bot, hệ thống sẽ tự động:

- Nhận yêu cầu từ Telegram
- Gửi prompt tới Google Gemini AI
- Sinh nội dung bài viết dạng HTML
- Tự động đăng bài lên WordPress

Mô hình hệ thống:
```
Telegram User
      ↓
Telegram Bot
      ↓
Telegram Trigger (n8n)
      ↓
Google Gemini AI
      ↓
Code JavaScript
      ↓
WordPress API
      ↓
Website WordPress
```

Cách hoạt động:
- Người dùng gửi nội dung tới Telegram Bot
- Telegram Trigger trong n8n nhận được tin nhắn
- Nội dung được gửi tới Google Gemini AI
- AI sinh tiêu đề và nội dung bài viết dạng HTML
- Node Code in JavaScript xử lý dữ liệu JSON trả về
- Node WordPress sử dụng API để đăng bài viết
- Bài viết xuất hiện trực tiếp trên website WordPress


---

## 2. CÁC BƯỚC THỰC HIỆN

### Bước 1: Cập nhật docker-compose.yml
#### Sửa file docker-compose thêm service n8n
- Vào thư mục dự án của bài tập 3 `wordpress-project`
- Sửa file `nano docker-compose.yml`
- Thêm service n8n vào file và lưu lại.

```
n8n:
    image: n8nio/n8n:latest
    container_name: wp_n8n
    restart: always
    ports:
      - "5678:5678"                            # Port mặc định của n8n
    environment:
      - TZ=Asia/Ho_Chi_Minh
      # Điền sub-domain đã cấu hình trỏ về service n8n này trong Cloudflare Dashboard vào đây:
      - WEBHOOK_URL=https://n8n.nguyentrunghiieu.id.vn/ 
    volumes:
      - ./n8n-data:/home/node/.n8n            # Lưu lại dữ liệu của n8n để không bị mất khi restart
```

#### Khởi động lại hệ thống
```
docker compose up -d
```
#### Kiểm tra các service
```
docker compose ps
```

<img width="2329" height="442" alt="image" src="https://github.com/user-attachments/assets/6bf07097-4989-4c7d-a961-8af1be86501d" />

### Bước 2: Cấu hình thêm route trên cloudflare tunnel
- Cấu hình Tunel cloudflare để truy cập vào các dịch vụ bằng các subdomain
- Thêm 2 route sau:

| Subdomain | Serivce URL |
| --- | --- |
| phpmyadmin.nguyentrunghiieu.id.vn | http://phpmyadmin:80 |
| n8n.nguyentrunghiieu.id.vn | http://n8n:5678 |

#### Thêm route cho phpmyadmin:

<img width="3071" height="1737" alt="image" src="https://github.com/user-attachments/assets/361a98ac-d2ff-458c-9b0a-818740628e76" />

#### Thêm route cho n8n:

<img width="3071" height="1743" alt="image" src="https://github.com/user-attachments/assets/110966e9-04aa-4164-8be3-d2c6b90c48c2" />

#### Thêm route hoàn tất:

<img width="3070" height="1745" alt="image" src="https://github.com/user-attachments/assets/90129b82-5223-4a33-9e36-1305ce217088" />

### Bước 3: Truy cập subdomain xem kết quả

#### Kết quả truy cập subdomain `https://wordpress.nguyentrunghiieu.id.vn/wp-admin`

<img width="3072" height="1920" alt="image" src="https://github.com/user-attachments/assets/a4a676b8-9499-4abc-a5a9-6850be59aeee" />

#### Kết quả truy cập subdomain `https://n8n.nguyentrunghiieu.id.vn/`

<img width="3072" height="1920" alt="image" src="https://github.com/user-attachments/assets/c46a861e-af30-4532-b51c-107489e23579" />

#### Kết quả truy cập subdomain `https://phpmyadmin.nguyentrunghiieu.id.vn/`

<img width="3072" height="1920" alt="image" src="https://github.com/user-attachments/assets/85065370-e6bb-415d-bf7e-6ee8148b3afd" />

### Bước 4: Tạo bài viết trên Wordpress

#### Bước 4.1. Bài viết giới thiệu bản thân

##### Bài viết 1: đã thực hiện ở bài tập 3
<img width="3069" height="1741" alt="image" src="https://github.com/user-attachments/assets/96de0fd2-5869-4e07-9986-32313dcea0b2" />

##### Bài viết 2: sử dụng html
<img width="3071" height="1736" alt="image" src="https://github.com/user-attachments/assets/a592c25c-7537-43bc-b239-2e152cf93dba" />

#### Bước 4.2. Bài viết giới thiệu những kiến thức đã học được ở môn học Phát triển ứng dụng với mã nguồn mở

<img width="3071" height="1745" alt="image" src="https://github.com/user-attachments/assets/66cfc697-7aa3-4545-9071-7f64b29710ec" />

<img width="3071" height="1742" alt="image" src="https://github.com/user-attachments/assets/90ed7926-2f7e-489a-9ee3-c50bc2095a83" />

### Bước 5: Cấu hình n8n

#### Truy cập subdomain `https://n8n.nguyentrunghiieu.id.vn/` để tạo tài khoản và kích hoạt license
##### Điền các thông tin , đặc biệt email cần chính xác -> Next

<img width="3064" height="1741" alt="image" src="https://github.com/user-attachments/assets/d46620e7-ac2e-4584-b0e5-8a62392f4e46" />

##### Chọn `Send me a free license key`

<img width="3068" height="1745" alt="image" src="https://github.com/user-attachments/assets/8224bd35-35c2-409c-a540-898fa7b71a2d" />

##### Trong Usage and plan -> Enter Activation Key -> Nhập Key lấy từ email -> Activate

<img width="3070" height="1735" alt="Screenshot 2026-05-24 100716" src="https://github.com/user-attachments/assets/da866e3b-8106-460a-ad6b-40392fa0ad45" />

#####  Nhận thông báo thành công: 
<img width="3067" height="1736" alt="image" src="https://github.com/user-attachments/assets/c6892a15-b190-4b33-9786-580269696d2a" />

#### Tạo workflow
##### Chọn Home page -> Overview -> New workflow

<img width="3067" height="1751" alt="image" src="https://github.com/user-attachments/assets/89b96aba-3b78-4eeb-aeb0-b220a00df6d2" />

##### Tạo thành công:
<img width="3071" height="1739" alt="image" src="https://github.com/user-attachments/assets/b52b321e-269f-4f6b-a501-aa2787ac2879" />


---

## 3. KẾT QUẢ ĐẠT ĐƯỢC

---

## 4. NHẬN XÉT

---
