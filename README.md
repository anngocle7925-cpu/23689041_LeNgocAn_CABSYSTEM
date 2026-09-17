# 23689041_LeNgocAn_CABSYSTEM
Dự án CAB System - Môn Lập trình hướng dịch vụ


# CAB System — Nền tảng đặt xe

## Tổng quan hệ thống

| Mục | Nội dung |
|---|---|
| **Tên dự án** | CAB System |
| **Loại hệ thống** | Nền tảng đặt xe trực tuyến (ride-hailing platform), tương tự mô hình Grab/Uber |
| **Khách hàng** | Công ty ABC — doanh nghiệp cung cấp dịch vụ đặt xe |
| **Thời gian triển khai** | 7 tuần |
| **Định hướng kiến trúc** | Service-Oriented Architecture / Microservices (do yêu cầu về khả năng mở rộng độc lập, triển khai từng phần, cô lập lỗi giữa các module) |
| **Phạm vi môn học** | Bài tập lớn môn Lập trình hướng dịch vụ (SOA) |

**Ý tưởng cốt lõi:** CAB System kết nối 3 nhóm người dùng — khách hàng, tài xế và nhân viên vận hành — thông qua một chuỗi nghiệp vụ xuyên suốt: *tạo yêu cầu đặt xe → tìm & phân công tài xế → thực hiện chuyến đi → tính cước & thanh toán → thông báo → đánh giá sau chuyến*. Hệ thống không chỉ là một ứng dụng đặt xe đơn thuần mà là một **nền tảng (platform)** có khả năng mở rộng thêm dịch vụ, phương thức thanh toán, kênh thông báo trong tương lai mà không phải xây dựng lại toàn bộ.

Vì hệ thống có nhiều nghiệp vụ độc lập nhưng liên kết chặt (đặt xe, matching, thanh toán, thông báo, quản trị), đây là bài toán rất phù hợp để thiết kế theo hướng **chia nhỏ thành các service riêng biệt**, giao tiếp với nhau qua API/message — đúng tinh thần môn SOA.

---

## Cấu trúc tài liệu

| File / Thư mục | Nội dung | Tuần |
|---|---|---|
| [`srs.md`](./srs.md) | Tài liệu phân tích nghiệp vụ đầy đủ (SRS) — 14 bước: từ tìm hiểu nghiệp vụ, stakeholder, phạm vi dự án, đến FR/NFR, ERD, Use Case, Sequence Diagram, Acceptance Criteria, Traceability Matrix | Tuần 1 |
| [`API_document.md`](./API_document.md) | Tài liệu tổng quan API: quy ước chung, danh mục 32 endpoint, mã lỗi chuẩn, hướng dẫn xem chi tiết | Tuần 2 |
| [`openapi.yaml`](./openapi.yaml) | Đặc tả OpenAPI 3.0 **gộp** toàn bộ API — import vào [Swagger Editor](https://editor.swagger.io) hoặc Postman để xem/test trực tiếp | Tuần 2 |
| [`API/`](./API) | 11 file OpenAPI **tách riêng theo từng resource** (auth, trip, payment, matching...) — dùng khi cần xem/sửa 1 phần cụ thể mà không mở file gộp | Tuần 2 |
