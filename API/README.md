# API — CAB System

Toàn bộ đặc tả API của nền tảng đặt xe CAB System, thiết kế theo kiến trúc SOA/microservices.

## Bắt đầu từ đâu?

- 📖 [`API_document.md`](./API_document.md) — tài liệu tổng quan: quy ước, danh mục 32 API, mã lỗi chuẩn
- 📄 [`openapi.yaml`](./openapi.yaml) — đặc tả OpenAPI 3.0 đầy đủ, import vào [Swagger Editor](https://editor.swagger.io) hoặc Postman để xem/test trực tiếp
- 📁 11 file `*-service.yaml` — đặc tả OpenAPI tách riêng theo từng resource (auth, trip, payment...), dùng khi cần xem/sửa 1 phần cụ thể mà không mở file gộp
- 📋 [`srs.md`](./srs.md) — tài liệu phân tích nghiệp vụ gốc (SRS), là nguồn của toàn bộ Functional Requirements dùng để thiết kế API này

## Quick start

1. Mở `openapi.yaml` bằng Swagger Editor để xem giao diện trực quan
2. Hoặc import vào Postman để test thử từng endpoint

## Danh sách 11 file OpenAPI theo resource

| File | Service | FR bao phủ |
|---|---|---|
| `auth-service.yaml` | Auth | FR-01, FR-02 |
| `customer-profile-service.yaml` | Customer Profile | FR-03 |
| `driver-profile-service.yaml` | Driver Profile | FR-04, FR-05 |
| `trip-service.yaml` | Trip | FR-07, FR-08, FR-09, FR-10, FR-12 |
| `trip-status-service.yaml` | Trip Status | FR-11, FR-21 |
| `trip-rating-service.yaml` | Trip Rating | FR-13 |
| `matching-service.yaml` | Matching *(internal)* | FR-14, FR-15, FR-16, FR-17 |
| `driver-ops-service.yaml` | Driver Operations | FR-18, FR-19, FR-20, FR-22 |
| `payment-service.yaml` | Payment | FR-23, FR-24, FR-25, FR-27, FR-28 |
| `notification-service.yaml` | Notification | FR-29, FR-30 |
| `admin-service.yaml` | Admin & Reporting | FR-32 → FR-37 |

`openapi.yaml` là bản gộp của cả 11 file trên thành 1 spec thống nhất (đã kiểm tra hợp lệ theo chuẩn OpenAPI 3.0).
