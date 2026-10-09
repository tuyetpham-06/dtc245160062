# Website Quảng bá Sản phẩm (WordPress) - Đồ án Triển khai & Quản trị HTPM

## 1. Giới thiệu
Hệ thống triển khai một Website giới thiệu sản phẩm bằng WordPress, đi kèm cơ sở dữ liệu MySQL và công cụ quản lý phpMyAdmin. Hệ thống được giám sát bằng Prometheus, Grafana, và quản lý log tập trung với Loki + Promtail. Toàn bộ kiến trúc được triển khai bằng Docker Compose.

## 2. Cấu trúc hệ thống
- **WordPress**: Nền tảng CMS chính.
- **MySQL**: Cơ sở dữ liệu.
- **phpMyAdmin**: Quản lý DB trực quan.
- **Nginx**: Reverse Proxy với các cấu hình Security Headers cơ bản.
- **Prometheus + Grafana**: Thu thập metrics và hiển thị Dashboard giám sát.
- **Loki + Promtail**: Thu thập và truy vấn Log tập trung.

## 3. Hướng dẫn chạy hệ thống

### Yêu cầu
- Đã cài đặt Docker và Docker Compose.

### Các bước thực hiện
1. Clone repository này về máy.
2. Mở terminal tại thư mục chứa file `docker-compose.yml`.
3. Chạy lệnh để khởi động toàn bộ dịch vụ:
   ```bash
   docker-compose up -d
   ```

### Thông tin truy cập
- **Website WordPress**: http://localhost
- **phpMyAdmin**: http://localhost:8080 (User: root / Pass: StrongRootPassword123!)
- **Prometheus**: http://localhost:9090
- **Grafana**: http://localhost:3000 (User: admin / Pass: AdminStrong123!)
- **Loki**: http://localhost:3100

## 4. Các truy vấn LogQL mẫu (Loki)
- Lấy log của container WordPress: `{container="wp_app"}`
- Lọc các log lỗi (error) từ Nginx: `{container="wp_nginx"} |= "error"`

## 5. Các biện pháp Hardening đã áp dụng
- Sử dụng mật khẩu mạnh cho DB và Grafana thông qua biến môi trường.
- Áp dụng `security_opt: no-new-privileges:true` để ngăn chặn leo thang đặc quyền trong container.
- Đưa các dịch vụ vào một `wp_network` chung (Network Isolation).
- Thiết lập Security Headers trong Nginx để chống Clickjacking và XSS (X-Frame-Options, X-XSS-Protection, ...).
