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

Sinh viên chọn **1 trong 2 loại web** sau:

---

### 🔹 4.1. WEB THƯƠNG MẠI ĐIỆN TỬ

- Web dạng **Single Page Application (SPA)**  
  → chỉ gồm **1 file `index.html`**, toàn bộ giao diện do **JavaScript sinh động**.  
- **Tính năng login:**  
  - Lưu phiên đăng nhập vào **cookie** và **session**.  
  - Thông tin login lưu trong **MariaDB**, được quản trị bằng **phpMyAdmin**.  
  - **Mã hoá dữ liệu login** khi gửi.  
  - Chỉ cần login **1 lần**, đăng xuất mới phải login lại.  
- **Tính năng trang chủ:** liệt kê **các sản phẩm bán chạy**.  
- **Tính năng nhóm sản phẩm:** liệt kê **các nhóm sản phẩm**.  
- **Tính năng sản phẩm theo nhóm.**  
- **Tính năng tìm kiếm sản phẩm.**  
- **Giỏ hàng:**  
  - Thêm sản phẩm vào giỏ.  
  - Thay đổi số lượng.  
  - Cập nhật tổng tiền.  
- **Đặt hàng:** nhập thông tin giao hàng → tạo **đơn hàng**.  
- **Tính năng Admin:**  
  - Thống kê số lượng đơn hàng.  
  - **Call** xác nhận & cập nhật trạng thái đơn hàng.  
  - Gửi thông tin cho bộ phận đóng gói, bưu điện.  
  - Cập nhật mã COD, tình trạng giao hàng, huỷ hàng,...  
- **Biểu đồ thống kê:**  
  - Thống kê **số lượng mặt hàng bán được theo ngày** (sử dụng **Grafana**).  
- **Backend:** sử dụng **Node-RED** xử lý request từ **JavaScript**, phản hồi về **JSON**.

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




