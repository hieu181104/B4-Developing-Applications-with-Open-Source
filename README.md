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

#### Bước 5.1. Truy cập subdomain `https://n8n.nguyentrunghiieu.id.vn/` để tạo tài khoản và kích hoạt license
##### Điền các thông tin , đặc biệt email cần chính xác -> Next

<img width="3064" height="1741" alt="image" src="https://github.com/user-attachments/assets/d46620e7-ac2e-4584-b0e5-8a62392f4e46" />

##### Chọn `Send me a free license key`

<img width="3068" height="1745" alt="image" src="https://github.com/user-attachments/assets/8224bd35-35c2-409c-a540-898fa7b71a2d" />

##### Trong Usage and plan -> Enter Activation Key -> Nhập Key lấy từ email -> Activate

<img width="3070" height="1735" alt="Screenshot 2026-05-24 100716" src="https://github.com/user-attachments/assets/da866e3b-8106-460a-ad6b-40392fa0ad45" />

#####  Nhận thông báo thành công: 
<img width="3067" height="1736" alt="image" src="https://github.com/user-attachments/assets/c6892a15-b190-4b33-9786-580269696d2a" />

#### Bước 5.2. Tạo workflow
##### Chọn Home page -> Overview -> New workflow

<img width="3067" height="1751" alt="image" src="https://github.com/user-attachments/assets/89b96aba-3b78-4eeb-aeb0-b220a00df6d2" />

##### Tạo thành công:
<img width="3071" height="1739" alt="image" src="https://github.com/user-attachments/assets/b52b321e-269f-4f6b-a501-aa2787ac2879" />

#### Bước 5.3. Tạo Telegram Bot

Mở Telegram và tìm kiếm `BotFather` có tích xanh chính chủ

#####  Bấm `/start`

<img width="3071" height="1603" alt="image" src="https://github.com/user-attachments/assets/583bf7f1-a807-4d5c-b3bf-c29bfafec628" />

##### Nhập lệnh `/newbot`

<img width="3071" height="1598" alt="image" src="https://github.com/user-attachments/assets/59619711-d003-46e9-a3a5-c50e9c9a2fe8" />

##### Đặt tên cho bot `Bot_n8n_wordpress`
##### Đặt usename cho bot, bắt buộc kết thúc bằng `bot` (`trunghieu_n8n_bot`)
##### Copy token HTTP API (màu vàng, 8825085837:AAEMXOaUr4t3VdfMr3gHMr3tNvaoqKQ8h5Q)

<img width="3070" height="1601" alt="image" src="https://github.com/user-attachments/assets/6a222994-d73b-4d49-acce-50f5be5d157e" />

##### Chat với bot mới lần đầu:

<img width="3071" height="1597" alt="image" src="https://github.com/user-attachments/assets/1e1e6f44-293c-455a-96c2-755666c13574" />

#### Bước 5.4. Cấu hình node Telegram trong workflow

##### Add trigger node: tìm node: Telegram -> OnMessage ; cấu hình Credential: Set up Credential -> cần Nhập Access Token

<img width="3071" height="1590" alt="image" src="https://github.com/user-attachments/assets/f1e60ab7-2a12-494c-a199-4933a68b1ecd" />

##### Dán token HTTP API từ BotFather gửi -> Save

<img width="3070" height="1592" alt="image" src="https://github.com/user-attachments/assets/5c9dbd36-901e-49cd-bec5-00caeeee4732" />

##### Nhấn Test this trigger để kiểm tra, vào Telegram Bot vừa tạo rồi gửi bất kỳ `helo bot`

<img width="3065" height="1589" alt="image" src="https://github.com/user-attachments/assets/6a136039-9228-46db-87c1-ef457ff4c6ec" />

##### Nếu kết nối thành công, output của node sẽ hiện nội dung 
<img width="3068" height="1592" alt="image" src="https://github.com/user-attachments/assets/311928f5-67a1-4a58-8ef9-490b1e5d5f57" />

#### Bước 5.5. Cấu hình node AI Google Gemini

##### Add (nối tiếp vào sau node Telegram Trigger) node: Google Gemini => Message a model

<img width="3071" height="1598" alt="Screenshot 2026-05-24 104815" src="https://github.com/user-attachments/assets/5e15077b-f30c-4770-8c7a-823818463078" />

##### Thực hiện Set up Credential -> Cần nhập Token API Key
- Lấy API KEY tại trang: https://aistudio.google.com => https://aistudio.google.com/api-keys
- Tạo project mới, rồi tạo API KEY

<img width="3067" height="1595" alt="image" src="https://github.com/user-attachments/assets/c450704b-2b01-4cbd-aaa7-ec5be507ed8a" />

- Nhập API key lên giao diện setup trong n8n rồi nhấn save.

<img width="3071" height="1595" alt="image" src="https://github.com/user-attachments/assets/74d1040a-ea5c-4db1-89e7-a7279b8e6bd8" />

- Kéo thả nội dung đã chát với bot của telegram (phía bên trái) vào nội dung phần PROMPT kết quả được {{ $json.message.text }}, cần gõ thêm vào sau {{ $json.message.text }} để promt dài hơn

```
{{ $json.message.text }}.Kết quả sinh ra ở định dạng HTML+CSS để tôi dùng HTML+CSS này tạo bài viết cho wordpress.
Trả về JSON với 2 trường:
- post_title
- post_content
```
- Turn on Output Content as JSON : để kết quả trả về dạng json

<img width="3071" height="1598" alt="image" src="https://github.com/user-attachments/assets/d75fc7dc-7512-4fb9-bf4e-7a2f44f391ca" />

#### Bước 5.6. Cấu hình node Code in JavaSript

- Add (nối tiếp vào sau node Message a model) node: Code in JavaScript
- Dán code sau:
```
// 1. lấy dữ liệu gốc
const rawText = $input.first().json.content.parts[0].text;

// 2. Làm sạch chuỗi JSON (Xóa bỏ các ký tự markdown ```json hoặc ``` nếu AI vô tình sinh ra)
let cleanText = rawText.trim();

// Xóa đoạn ```json ở đầu nếu có
if (cleanText.startsWith("```json")) {
  cleanText = cleanText.substring(7);
} else if (cleanText.startsWith("```")) {
  cleanText = cleanText.substring(3);
}

// Xóa đoạn ``` ở cuối nếu có
if (cleanText.endsWith("```")) {
  cleanText = cleanText.substring(0, cleanText.length - 3);
}

cleanText = cleanText.trim();

// 3. Chuyển đổi chuỗi thành Object trong JavaScript
const cleanData = JSON.parse(cleanText);

// 4. Trả về kết quả định dạng lại gọn gàng cho node WordPress sử dụng
return {
  title: cleanData.post_title,
  content: cleanData.post_content
};
```

##### Test output node:
<img width="3069" height="1606" alt="image" src="https://github.com/user-attachments/assets/a0e3bd15-cfef-4e28-b8f9-dc8a0ff43985" />

#### Bước 5.7. Cấu hình node WordPress

##### - Add (nối tiếp vào sau node Code in JavaScript) node: WordPress => Create a Post
##### - Lấy "Mật khẩu ứng dụng" (Application Password) từ WordPress
- Truy cập trang quản trị: `https://wordpress.nguyentrunghieu.id.vn/wp-admin`
- Vào `Tài khoản` -> chọn user lúc thiết lập -> Mật khẩu ứng dụng -> Nhập tên `n8n` -> Thêm mật khẩu ứng dụng

<img width="3071" height="1594" alt="image" src="https://github.com/user-attachments/assets/09e73403-6cde-4325-b56f-3fe8ab0b21a6" />

<img width="2684" height="663" alt="image" src="https://github.com/user-attachments/assets/7a5c1b04-0a4b-479f-a0a6-d76d03752c4d" />

#####  Copy mật khẩu rồi dán vào password của n8n credential, điền các thông tin -> save
- Ignore SSL Issues (Insecure): TURN ON
- Wordpress URL: điền giá trị `https://wordpress.nguyentrunghiieu.id.vn/` (subdomain1)

<img width="3063" height="1597" alt="image" src="https://github.com/user-attachments/assets/9710475f-4b48-4572-8aff-978761a2e6fe" />

##### Mapping dữ liệu
- Kéo thả tiêu đề (Title): Nhìn sang cột dữ liệu bên trái của node JavaScript trước đó, bạn sẽ thấy cột title. Nhấp giữ chuột vào trường title này và kéo thả trực tiếp vào ô nhập liệu Title của node WordPress. (Nó sẽ tự động điền mã biểu thức dạng {{ $json.title }}).
- Kéo thả nội dung (Content): Tương tự, nhấp giữ chuột vào trường content ở cột bên trái và kéo thả vào ô nhập liệu Content của node WordPress. (Nó sẽ tự động điền {{ $json.content }}).
- Cấu hình Trạng thái xuất bản (Status):

Nhấp vào nút Add Field (Thêm thuộc tính) ở cuối cấu hình.
  - Chọn thuộc tính Status.
  - Ở ô giá trị của Status, chọn là Publish (để bài viết hiển thị công khai trên web ngay lập tức thay vì nằm ở mục Bản nháp - Draft).

<img width="3071" height="1600" alt="image" src="https://github.com/user-attachments/assets/ed0812b8-15b1-4a15-ad3f-3ca79d6b40b6" />

##### Workflow hoàn chỉnh

<img width="3071" height="1745" alt="image" src="https://github.com/user-attachments/assets/2f922b06-d298-4b02-8e7f-183e3ec5e422" />

#### Bước 5.8. PUBLISH FLOW
Nút này thực hiện việc xuất bản flow <=> flow sẽ tự động thực thi khi thoả mãn điều kiện trigger

<img width="3062" height="1589" alt="image" src="https://github.com/user-attachments/assets/be6b955b-327b-46aa-9e9b-1d25a0edd643" />

---

## 3. KẾT QUẢ ĐẠT ĐƯỢC
### 3.1. Kết quả demo
#### Chat với tele bot với yêu cầu : `Viết cho tôi một bài viết giới thiệu các quán ăn nổi tiếng ở Hà Nội, Việt Nam`

<img width="3066" height="1590" alt="image" src="https://github.com/user-attachments/assets/b0636b80-5d4a-4cd7-a765-c8083544811d" />

#### Flow n8n:

<img width="3071" height="1672" alt="image" src="https://github.com/user-attachments/assets/f5800ca1-b884-4b2b-a57f-f4bea5e97e0e" />

#### Bài đăng trên Wordpress

<img width="3071" height="1838" alt="image" src="https://github.com/user-attachments/assets/5903968a-c4c6-4d77-bc42-c32f06d49104" />

### 3.2. Thực hiện yêu cầu mới
#### Chat với bot: `Viết cho tôi một bài viết về các ngành học hot nhất năm 2025.`

<img width="3071" height="1601" alt="image" src="https://github.com/user-attachments/assets/cc1c369e-693d-4a01-886b-f4f4b8af453d" />

#### Bài viết trên wordpress:

<img width="3072" height="1920" alt="image" src="https://github.com/user-attachments/assets/8833846d-e07c-4e07-b25d-32b659b32e15" />

---

## 4. NHẬN XÉT

### 4.1. Quy trình vận hành hệ thống

Hệ thống hoạt động theo mô hình khép kín tự động hóa hoàn toàn:

- Yêu cầu đầu vào: Người dùng gửi yêu cầu viết bài bằng tiếng Việt qua Telegram Bot của cá nhân.

- Kích hoạt & Xử lý: Node Telegram Trigger trên n8n nhận tin nhắn và chuyển tiếp tới Google Gemini AI.

- Sinh nội dung: Gemini AI nhận diện ngữ cảnh, tự động thiết kế bài viết chuẩn định dạng HTML/CSS và đóng gói dưới dạng cấu trúc JSON.

- Lọc dữ liệu: Node Code JavaScript lọc sạch các ký tự markdown thừa, chuẩn hóa hai trường dữ liệu title và content.

- Xuất bản: Node WordPress kết nối qua Application Password bảo mật, tự động tạo và xuất bản bài viết trực tiếp lên website.

### 4.2. Những điều đã đạt được & Kiến thức tích lũy

- Triển khai hạ tầng Container: Làm chủ kỹ thuật cấu trúc file docker-compose.yml để chạy đồng thời nhiều service liên kết chặt chẽ (MariaDB, WordPress, phpMyAdmin, n8n, Cloudflared).

- Tự động hóa & Tích hợp AI: Làm quen với nền tảng n8n, cách xây dựng kịch bản (workflow) tích hợp các API hiện đại như Telegram API, Google Gemini API và WordPress Rest API.

- Xử lý dữ liệu: Ứng dụng JavaScript cơ bản để xử lý, làm sạch chuỗi và phân tích cú pháp (parse JSON) thực tế, khắc phục triệt để lỗi phân quyền (permission) dữ liệu trong Docker.

### 4.3. Những vấn đề tồn tại & Hướng phát triển

Tính ổn định của AI: Đôi khi Gemini AI có thể phản hồi sai cấu trúc JSON mong muốn dẫn đến lỗi phân tích cú pháp ở node JS (đã được khắc phục tạm thời bằng mã xử lý lỗi nâng cao).

Quản lý tài nguyên: Việc chạy nhiều service nặng trên môi trường ảo hóa Ubuntu cần được tối ưu cấu hình để tránh hiện tượng tràn RAM.

Hướng phát triển: Tích hợp thêm AI tạo ảnh (như Imagen) để tự động sinh ảnh đại diện (Featured Image) cho bài viết WordPress giúp bài đăng sinh động hơn.


---
