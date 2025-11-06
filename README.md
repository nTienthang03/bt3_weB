# bt3_weB

# 🚀 LẬP TRÌNH ỨNG DỤNG WEB TRÊN NỀN LINUX

## 🧰 CÀI ĐẶT MÔI TRƯỜNG LINUX

Sinh viên chọn **1 trong các phương án** sau:

- Enable WSL: cài đặt Docker Desktop  
- Enable WSL: cài đặt Ubuntu  
- Sử dụng Hyper-V: cài đặt Ubuntu  
- Sử dụng VMware: cài đặt Ubuntu  
- Sử dụng VirtualBox: cài đặt Ubuntu  

---

## 🐳 CÀI ĐẶT DOCKER

> Nếu dùng Docker Desktop trên Windows thì **Docker đã có sẵn**.

Sử dụng **1 file `docker-compose.yml`** để cài đặt các Docker container sau:

| Container | Cổng | Ghi chú |
|------------|------|---------|
| mariadb | 3306 | Cơ sở dữ liệu |
| phpmyadmin | 8080 | Quản trị MariaDB |
| nodered/node-red | 1880 | Xử lý backend |
| influxdb | 8086 | Lưu trữ dữ liệu IoT |
| grafana/grafana | 3000 | Hiển thị biểu đồ |
| nginx | 80, 443 | Web server |

---

## 💻 LẬP TRÌNH WEB FRONTEND + BACKEND

EM chọn * loại web** sau:

---


### 🔹 4.2. WEB IOT: GIÁM SÁT DỮ LIỆU IOT

- Web dạng **Single Page Application (SPA)**  
  → chỉ gồm **1 file `index.html`**, toàn bộ giao diện do **JavaScript sinh động**.  
- **Tính năng login:**  
  - Lưu phiên đăng nhập vào **cookie** và **session**.  
  - Thông tin login lưu trong **MariaDB**, được quản trị bằng **phpMyAdmin**.  
  - **Mã hoá dữ liệu login** khi gửi.  
  - Chỉ cần login **1 lần**, đăng xuất mới phải login lại.  
- **Hiển thị dữ liệu cảm biến:**  
  - Hiển thị **giá trị mới nhất** của các thông số giám sát.  
  - Khi **click** vào → hiển thị **đồ thị lịch sử** (nhúng **Grafana iframe**).  
- **Backend:**  
  - Sử dụng **Node-RED** để đọc dữ liệu từ cảm biến (hoặc API online).  
  - **Lưu dữ liệu mới nhất (update)** vào **MariaDB**.  
  - **Lưu lịch sử (insert)** vào **InfluxDB** → phục vụ **Grafana** hiển thị biểu đồ.  
  - Quản trị bảng lần đầu bằng **phpMyAdmin**.  

---

## 🌐 CẤU HÌNH NGINX

- Làm **web-server** chính.  
- Cấu hình để chạy website qua URL:  


# 1. 🐳 CHỌN PHƯƠNG ÁN DOCKER DESKTOP + WSL2

---

## 1.1. BẬT WSL2 (Windows Subsystem for Linux)

### 🔧 Thực hiện:
1. Mở **PowerShell (Run as Administrator)**  
2. Chạy lệnh sau để cài đặt WSL và Ubuntu mặc định:
   ```bash
   wsl --install
Kiểm tra phiên bản WSL:
wsl -l -v

3  Nếu chưa phải WSL2, chuyển đổi sang WSL2 bằng lệnh:

wsl --set-default-version 2

<img width="1920" height="1080" alt="Screenshot (236)" src="https://github.com/user-attachments/assets/1f4756a4-af54-4a5f-96f0-81bb02408dba" />

4 Cài Ubuntu
Nếu WSL chưa cài Ubuntu: wsl --install -d Ubuntu-22.04

Sau khi cài xong:

Chạy Ubuntu (gõ “Ubuntu” trong Start)

<img width="1920" height="1080" alt="Screenshot (237)" src="https://github.com/user-attachments/assets/743b80fa-9076-42ea-82bb-f67f2a0a10bf" />

#  2 Cài đặt Docker

bước 1 dowload 

<img width="1462" height="487" alt="image" src="https://github.com/user-attachments/assets/6cc8ed45-7518-4761-a7b1-e021b5eb4a44" />

bước 2 setting 

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/96062299-e049-42af-9093-c98d53dc1d38" />

bước 3 Kiểm tra Docker trong Ubuntu

trạng thái : Đã hoạt động

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/703eb1cc-1a2a-4b3a-a784-bc7ad879af86" />

 # DỰNG HỆ THỐNG DOCKER BẰNG FILE docker-compose.yml

 cd /mnt/d

mkdir baitap3_web

cd baitap3_web

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/e9bed7f0-ad81-4d02-a29a-5351104c5cb7" />

 Tạo file docker-compose.yml
 
 lệnh : nano docker-compose.yml
 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/89a8bbbc-575c-4953-8d55-d39031fe118b" />

Sao chép toàn bộ nội dung bên dưới
```version: "3.8"

services:
  mariadb:
    image: mariadb:10.6
    container_name: mariadb
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: root
      MYSQL_DATABASE: webdb
    ports:
      - "3306:3306"
    volumes:
      - mariadb_data:/var/lib/mysql

  phpmyadmin:
    image: phpmyadmin/phpmyadmin
    container_name: phpmyadmin
    restart: always
    environment:
      PMA_HOST: mariadb
      PMA_USER: root
      PMA_PASSWORD: root
    ports:
      - "8080:80"
    depends_on:
      - mariadb

  nodered:
    image: nodered/node-red
    container_name: nodered
    restart: always
    ports:
      - "1880:1880"
    volumes:
      - nodered_data:/data

  influxdb:
    image: influxdb:1.8
    container_name: influxdb
    restart: always
    ports:
      - "8086:8086"
    volumes:
      - influxdb_data:/var/lib/influxdb

  grafana:
    image: grafana/grafana
    container_name: grafana
    restart: always
    ports:
      - "3000:3000"
    depends_on:
      - influxdb
    environment:
      - GF_SECURITY_ADMIN_USER=admin
      - GF_SECURITY_ADMIN_PASSWORD=admin

  nginx:
    image: nginx:latest
    container_name: nginx
    restart: always
    ports:
      - "80:80"
      - "443:443"
    volumes:
      - ./nginx.conf:/etc/nginx/nginx.conf:ro
      - ./frontend:/usr/share/nginx/html

volumes:
  mariadb_data:
  influxdb_data:
  nodered_data:
```
Nhấn Ctrl + O → Enter để lưu

Nhấn Ctrl + X để thoát khỏi nano


<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/20947ba6-1c02-4ebf-b355-7a12c77a42ef" />

3.3 Tạo file nginx.conf

Trong thư mục /mnt/d/baitap3_web, gõ lệnh: nano nginx.conf
```
http {
  server {
    listen 80;
    server_name nthangi.com;

    # Trang web chính (Frontend)
    location / {
      root /usr/share/nginx/html;
      index index.html;
    }

    # Truy cập Node-RED qua http://nthangi.com/nodered
    location /nodered/ {
      proxy_pass http://nodered:1880/;
      proxy_set_header Host $host;
      proxy_set_header X-Real-IP $remote_addr;
    }

    # Truy cập Grafana qua http://nthangi.com/grafana
    location /grafana/ {
      proxy_pass http://grafana:3000/;
      proxy_set_header Host $host;
      proxy_set_header X-Real-IP $remote_addr;
    }
  }
}
``
Nhấn Ctrl + O → Enter để lưu

Nhấn Ctrl + X để thoát
Làm tương tự để ra được các file
```
<img  width="1817" height="488" alt="image" src="https://github.com/user-attachments/assets/64b47c37-576d-498f-b094-163a317c46f0" />


Sau đó kiểm tra các đường dẫn 

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/3f597097-4363-4026-81c7-35b307521279" />

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/be69bf2f-2409-45af-b0d2-954727eddad3" />

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/1c623dce-f481-44c3-9542-f0a55f5ce8db" />

# 4. LẬP TRÌNH WEB FRONTEND + BACKEND (WEB IoT)

# Mục tiêu:

Tạo một web IoT giám sát nhiệt độ – độ ẩm realtime:

Node-RED sinh dữ liệu cảm biến (giả lập).

Node-RED lưu vào InfluxDB để hiển thị biểu đồ.

Frontend index.html gọi API từ Node-RED, hiển thị thông tin hiện tại.

Grafana vẽ biểu đồ trực quan từ dữ liệu InfluxDB.


# 4.1 Cấu hình Node-RED (Backend API)
Mở Node-RED
Truy cập: http://localhost:1880

<img width="1919" height="1078" alt="image" src="https://github.com/user-attachments/assets/2b72461c-94b9-4758-a660-d8471a7ef7b2" />

Cài thêm các node cần thiết
Vào menu → Manage palette → Install

Tìm và cài 3 gói:

node-red-contrib-influxdb

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/f9c88dd4-6cad-4fbc-a137-696cd419b3b0" />

node-red-dashboard

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/b7237821-3697-44d3-9e95-6395654ad4d4" />

node-red-node-random

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/990bce56-55ed-4908-aa75-5789989321ef" />

# Tạo Flow mới

Chọn tab mới và tạo các node như sau:

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/53d7ff08-9fbb-48b5-a221-64f0518bc7a9" />

# Inject – “Cập nhật cảm biến (1s)”

Kiểu: inject

Tên: Cập nhật cảm biến (1s)

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/4d59a958-1b57-43e4-92b7-17ec12b2c431" />

Repeat: Every 1 second

Output: timestamp

Chức năng: Kích hoạt tự động mỗi giây để sinh dữ liệu cảm biến giả.


b. Function – “Sinh dữ liệu giả (sensors)”

Kiểu: function

Tên: Sinh dữ liệu giả (sensors)

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/a59908df-8828-4191-9503-13096b282834" />


c. InfluxDB Out – “Ghi dữ liệu cảm biến”
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/d6219aec-c893-41f2-9a57-c1b9be769412" />

Kiểu: influxdb out

Tên: Ghi dữ liệu cảm biến

Server: [v1.x] influxdb

Database: iot_data

Measurement: sensors

Chức năng: Ghi dữ liệu sensor sinh ra vào database iot_data

d. HTTP In – “API - GET /api/sensor”

Kiểu: http in

Tên: API - GET /api/sensor

Method: GET

URL: /api/sensor

Chức năng: Tạo endpoint API để client (frontend) truy vấn dữ liệu cảm biến.

<img width="1897" height="1079" alt="image" src="https://github.com/user-attachments/assets/00ac2ccb-d60b-4882-bae4-81c2cd37b523" />

e. Function – “Tạo query”

Kiểu: function

Tên: Tạo query

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/108ea0c5-249d-40a5-a93b-81ea246b6182" />


f. InfluxDB In – “Đọc Influx (v1.x)”

Kiểu: influxdb in

Server: [v1.x] influxdb

Database: iot_data

Query: lấy từ msg.query

Chức năng: Truy vấn dữ liệu cảm biến từ InfluxDB theo câu query đã tạo.

g. Function – “Trả JSON + CORS”

Kiểu: function

Tên: Trả JSON + CORS


<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/8e6bc0f3-afda-4188-9aef-e7dace0d4ea7" />


# Kết quả sau khi chạy 

<img width="1218" height="654" alt="image" src="https://github.com/user-attachments/assets/370af10f-ca2f-444d-8cc1-57e6cc9de3ab" />


### 4.2 Kết nối Grafana và hiển thị biểu đồ
a. Đăng nhập Grafana

Truy cập: http://localhost:3000
• Username: admin
• Password: admin (sau đó nhập mật khẩu mới)
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/179b96e2-1727-4743-9d75-ab833e832ce6" />



b. Thêm nguồn dữ liệu (Data Source)

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/95e410dd-81bd-4f63-8f62-6edc75eafa84" />

Ở menu bên trái → Connections → Data sources
Chọn InfluxDB
Cấu hình như sau:
URL: http://influxdb:8086
Database: iot_data
Query Language: InfluxQL
User: root
Password: 12456
Nhấn Save & Test

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/4d7c6357-2b6b-44b1-abe1-c418524f1289" />
kết quả 

<img width="1172" height="507" alt="image" src="https://github.com/user-attachments/assets/a97fea1a-2b93-47aa-969a-ea2d49151d01" />

 
<img width="1412" height="507" alt="image" src="https://github.com/user-attachments/assets/4ae13f3d-d884-493e-8476-c1062ccdefb6" />


# 4.3 Tạo Frontend (index.html)

<img width="1415" height="573" alt="image" src="https://github.com/user-attachments/assets/5e5ca7df-70ac-42d8-800c-025ab4b7ec76" />



 
 # 4.4 Kết quả ( )
 lưu ý : +thực hiện liên tục đễ fixx hết lỗi các bước hướng dẫn trên bằng CHAT GPT và GROCK AI để được kết quả như ảnh ) 
 

<img width="1624" height="1031" alt="image" src="https://github.com/user-attachments/assets/2d00294b-1ca9-4c28-9388-1295ec775730" />

## 💡 KIẾN THỨC VÀ KỸ NĂNG HỌC ĐƯỢC

### 1️⃣ **Hiểu và triển khai môi trường phát triển trên nền Linux**
- Biết cách **cài đặt và kích hoạt WSL2** trên Windows.  
- Biết cách **cài Ubuntu và Docker Desktop**, đồng thời chạy lệnh kiểm tra container.  
- Nắm rõ cách quản lý hệ thống qua **terminal Linux**, bao gồm khởi động, dừng, và kiểm tra trạng thái dịch vụ.

### 2️⃣ **Sử dụng Docker và Docker Compose**
- Học được cách tạo file `docker-compose.yml` để **quản lý nhiều container đồng thời**.  
- Hiểu rõ vai trò của từng container:  
  - **MariaDB**: Lưu trữ dữ liệu người dùng, sản phẩm hoặc cảm biến.  
  - **phpMyAdmin**: Quản trị cơ sở dữ liệu qua giao diện web.  
  - **Node-RED**: Xây dựng luồng xử lý backend và API JSON.  
  - **InfluxDB & Grafana**: Lưu trữ và hiển thị dữ liệu dạng thời gian thực.  
  - **Nginx**: Làm web server và reverse proxy cho toàn hệ thống.

### 3️⃣ **Triển khai ứng dụng web SPA**
- Biết cách xây dựng **index.html duy nhất**, kết hợp **JavaScript, HTML, CSS** để sinh giao diện động.  
- Sử dụng **Fetch API hoặc AJAX** để gọi dữ liệu JSON từ Node-RED.  
- Hiểu khái niệm **cookie, session** và cách lưu phiên đăng nhập của người dùng.  
- Biết cách **tạo form đăng nhập, tìm kiếm, hiển thị sản phẩm hoặc dữ liệu cảm biến** theo thời gian thực.

### 4️⃣ **Cấu hình Nginx làm Web Server**
- Học được cách cấu hình **domain tùy chỉnh (http://nguyentienthang.com)**.  
- Biết cấu hình **reverse proxy** để ánh xạ các dịch vụ khác như Node-RED (port 1880) và Grafana (port 3000) về cổng 80.  
- Hiểu quy trình **chuyển hướng HTTP → HTTPS** khi triển khai thật.

### 5️⃣ **Hiểu mối liên hệ giữa Frontend – Backend – Database**
- Nắm được mô hình **client – server** trong ứng dụng web.  
- Biết cách thiết kế API và gọi dữ liệu từ frontend bằng JSON.  
- Thực hành **tạo – đọc – ghi – sửa – xóa (CRUD)** dữ liệu qua Node-RED.

### 6️⃣ **Kỹ năng tổng hợp và báo cáo**
- Biết cách **tổ chức mã nguồn**, lưu trên **GitHub** và viết **README.md** rõ ràng.  
- Rèn luyện kỹ năng **mô tả quy trình triển khai** và **đưa minh chứng bằng hình ảnh**.  
- Học được cách viết báo cáo kỹ thuật chuyên nghiệp và có tính hệ thống.

---


- Cách triển khai hệ thống web hiện đại theo mô hình container hóa.  
- Tư duy kết nối giữa các dịch vụ backend và frontend trong một hệ thống thực tế.  
- Cách sử dụng các công cụ DevOps cơ bản như Docker, Nginx và Grafana.  

Đây là một bài tập giúp tôi rèn luyện **tư duy triển khai hệ thống hoàn chỉnh**, thay vì chỉ lập trình đơn lẻ.  
Tôi nhận thấy việc hiểu và làm chủ môi trường triển khai quan trọng không kém so với viết mã nguồn, vì nó quyết định khả năng **chạy được – chạy ổn định – và mở rộng hệ thống** sau này.




