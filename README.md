# BÀI TẬP MÔN LẬP TRÌNH WEB

- **Họ và tên:** Đào Tuấn Hưng
- **Lớp:** K59KMT.K01
- **Mã số sinh viên:** K235480106032
- **Trường:** Đại học Kỹ thuật Công Nghiệp Thái Nguyên - TNUT 

---

## BÀI TẬP 1: DEPLOY MULTI-SERVICE WEB STACK TRÊN DOCKER

### 1. Kiến trúc hệ thống
Hệ thống sử dụng **Docker Compose** triển khai đồng thời 5 dịch vụ:
- **Nginx (Reverse Proxy):** Lắng nghe cổng 80/443, định tuyến tên miền Virtual Host (`site1.hungtuan.id.vn`, `site2.hungtuan.id.vn`).
- **Node-RED:** Nền tảng IoT/API Engine chạy tại cổng 1880.
- **MariaDB:** Cơ sở dữ liệu quan hệ cho toàn bộ hệ thống (cổng 3306).
- **phpMyAdmin:** Công cụ quản trị CSDL trực quan trên trình duyệt (cổng 8080).
- **Cloudflared:** Dịch vụ Cloudflare Tunnel kết nối môi trường local ra internet.

---

## BÀI TẬP 2: XÂY DỰNG REST API VỚI NODE-RED & TÍNH NĂNG FETCH FRONTEND

### 1. Khởi tạo API trên Node-RED
- Nút `http in` nhận Request `GET /api/tacke`
- Nút `function` định dạng header JSON và trả về dữ liệu danh sách sinh viên.
- Nút `http response` gửi dữ liệu về client.

### 2. Định tuyến Reverse Proxy cho API tại Nginx
Nginx được bổ sung cấu hình chuyển tiếp `/api/` về Node-RED nhằm giải quyết bài toán CORS khi Frontend gọi API:
```nginx
location /api/ {
    proxy_pass http://nodered:1880/api/;
    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;
}


