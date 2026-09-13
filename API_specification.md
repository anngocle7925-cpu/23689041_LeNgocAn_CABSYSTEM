# CAB System API Specification

## 1. Tổng quan

CAB System là nền tảng đặt xe trực tuyến, hỗ trợ các nghiệp vụ:
- Quản lý tài khoản khách hàng và tài xế
- Đặt xe và quản lý chuyến
- Tìm và phân công tài xế
- Theo dõi trạng thái và vị trí chuyến
- Tính cước và thanh toán
- Gửi thông báo
- Đánh giá tài xế
- Quản trị, vận hành và báo cáo

API được thiết kế theo chuẩn RESTful và sử dụng HTTP/HTTPS để giao tiếp giữa client và hệ thống.

## 2. Base URL

```text
http://localhost:8080/api/v1

