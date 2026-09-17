# API Document — CAB System

## 1. Giới thiệu

Tài liệu này mô tả tổng quan toàn bộ API của **CAB System** — nền tảng đặt xe được thiết kế theo kiến trúc hướng dịch vụ (SOA/microservices). Tài liệu dành cho:

- Dev frontend (mobile app khách hàng/tài xế, web admin) — cần biết endpoint nào để gọi
- Tester/QA — cần biết request/response mẫu và các mã lỗi để viết test case
- Dev các service khác — cần biết API nội bộ nào có thể gọi qua lại
- Giảng viên/người đánh giá — cần thấy được bức tranh tổng thể và cách API bám sát yêu cầu nghiệp vụ (FR) đã phân tích ở `srs.md`

Tài liệu này là **tổng quan**. Chi tiết đầy đủ từng API (request/response schema, mọi mã lỗi, ví dụ) nằm trong file **`openapi.yaml`** — đây là nguồn thông tin chính xác và cập nhật nhất (single source of truth), tài liệu này không lặp lại để tránh sai lệch khi có thay đổi.

---

## 2. Kiến trúc & quy ước chung

### 2.1. Base URL theo môi trường

| Môi trường | URL |
|---|---|
| Production (giả lập) | `https://api.cabsystem.local/api/v1` |
| Local dev | `http://localhost:8080/api/v1` |
| Mạng nội bộ (service-to-service) | `http://internal-network:9000` |

### 2.2. Versioning

Tất cả API public đều có tiền tố phiên bản `/api/v1/...`. Khi có thay đổi phá vỡ tương thích ngược (breaking change), phiên bản mới sẽ là `/api/v2/...`, giữ song song `/v1` cho client cũ trong giai đoạn chuyển tiếp.

### 2.3. Ba loại API theo tiền tố đường dẫn

| Tiền tố | Ý nghĩa | Ai được gọi |
|---|---|---|
| *(không tiền tố, vd. `/trips`)* | **Public** — expose qua API Gateway | Khách hàng, Tài xế (qua app) |
| `/internal/...` | **Internal** — giao tiếp service-to-service | Chỉ các service trong hệ thống gọi lẫn nhau, không lộ ra ngoài Gateway |
| `/admin/...` | **Admin** — dành cho vận hành | Nhân viên vận hành (role `operator`/`admin`) |

### 2.4. Xác thực (Authentication)

| Loại API | Cơ chế xác thực | Header |
|---|---|---|
| Public, Admin | Bearer Token (JWT) | `Authorization: Bearer <access_token>` |
| Internal (service-to-service) | API Key nội bộ | `X-Internal-Api-Key: <key>` |
| Webhook (cổng thanh toán gọi vào) | Chữ ký số | `X-Gateway-Signature: <signature>` |

Tất cả request (trừ `/auth/register` và `/auth/login`) đều yêu cầu 1 trong 3 cơ chế trên. Header `Content-Type: application/json` là bắt buộc cho mọi request có body.

---

## 3. Danh mục toàn bộ API

| Method | Endpoint | Mô tả | FR liên quan | Service sở hữu |
|---|---|---|---|---|
| POST | `/auth/register` | Đăng ký tài khoản khách hàng | FR-01 | Auth |
| POST | `/auth/login` | Đăng nhập (khách hàng & tài xế) | FR-02 | Auth |
| GET | `/customers/me` | Xem hồ sơ khách hàng hiện tại | FR-03 | Customer Profile |
| PUT | `/customers/me` | Cập nhật hồ sơ khách hàng | FR-03 | Customer Profile |
| POST | `/drivers/register` | Đăng ký / tạo tài khoản tài xế | FR-04 | Driver Profile |
| GET | `/drivers/me` | Xem hồ sơ & phương tiện tài xế | FR-05 | Driver Profile |
| PUT | `/drivers/me` | Cập nhật hồ sơ & phương tiện tài xế | FR-05 | Driver Profile |
| POST | `/trips` | Đặt xe (tạo yêu cầu chuyến đi) | FR-07, FR-08, FR-09 | Trip |
| GET | `/trips` | Xem lịch sử chuyến đi | FR-12 | Trip |
| GET | `/trips/{tripId}` | Xem chi tiết / theo dõi chuyến real-time | FR-10 | Trip |
| POST | `/trips/{tripId}/cancel` | Hủy chuyến đi | FR-11 | Trip Status |
| PATCH | `/trips/{tripId}/status` | (Tài xế) Cập nhật trạng thái chuyến | FR-21 | Trip Status |
| POST | `/trips/{tripId}/rating` | Đánh giá tài xế sau chuyến | FR-13 | Trip Rating |
| POST | `/internal/matching/requests` | Yêu cầu tìm tài xế cho 1 chuyến | FR-14, FR-15 | Matching *(internal)* |
| GET | `/internal/matching/requests/{requestId}` | Truy vấn trạng thái matching | FR-17 | Matching *(internal)* |
| POST | `/internal/matching/requests/{requestId}/driver-response` | Ghi nhận phản hồi tài xế cho matching | FR-16 | Matching *(internal)* |
| PATCH | `/drivers/me/availability` | Bật/tắt trạng thái sẵn sàng | FR-18 | Driver Operations |
| GET | `/drivers/me/trip-offers/{offerId}` | Xem đề xuất chuyến đi | FR-19 | Driver Operations |
| POST | `/drivers/me/trip-offers/{offerId}/respond` | Chấp nhận / từ chối đề xuất chuyến | FR-20 | Driver Operations |
| POST | `/drivers/me/location` | Gửi vị trí hiện tại | FR-22 | Driver Operations |
| POST | `/payments` | Tạo yêu cầu thanh toán | FR-23, FR-24 | Payment |
| GET | `/payments/{paymentId}` | Xem trạng thái giao dịch thanh toán | FR-28 | Payment |
| POST | `/payments/{paymentId}/retry` | Thử lại thanh toán thất bại | FR-27 | Payment |
| POST | `/internal/payments/webhook` | Webhook nhận kết quả từ cổng thanh toán | FR-25 | Payment *(internal)* |
| POST | `/internal/notifications` | Gửi thông báo (service khác gọi vào) | FR-29, FR-30 | Notification *(internal)* |
| GET | `/notifications` | Xem lịch sử thông báo của bản thân | *(bonus, không thuộc FR)* | Notification |
| GET | `/admin/customers` | Tìm kiếm / liệt kê khách hàng | FR-32 | Admin |
| PUT | `/admin/customers/{customerId}` | Chỉnh sửa thông tin khách hàng | FR-32 | Admin |
| GET | `/admin/drivers` | Tìm kiếm / liệt kê tài xế | FR-33 | Admin |
| PUT | `/admin/drivers/{driverId}/verify` | Duyệt hồ sơ & phương tiện tài xế | FR-33 | Admin |
| PUT | `/admin/drivers/{driverId}/lock` | Khóa / mở khóa tài khoản tài xế | FR-33, FR-37 | Admin |
| GET | `/admin/trips/active` | Giám sát chuyến đang diễn ra | FR-34 | Admin |
| PUT | `/admin/users/{userId}/role` | Phân quyền nhân viên vận hành | FR-35 | Admin |
| GET | `/admin/reports` | Xem báo cáo tổng hợp vận hành | FR-36 | Admin |
| GET | `/admin/audit-logs` | Xem nhật ký thao tác quản trị | FR-37 | Admin |

**Tổng cộng: 32 endpoint**, bao phủ 31/34 FR chức năng (không tính FR-06, FR-26, FR-31 — xem mục 6).

---

## 4. Response Code & Error Code chuẩn dùng chung

### 4.1. HTTP Response Code

| Code | Ý nghĩa | Dùng khi nào |
|---|---|---|
| `200 OK` | Thành công | GET, PUT, PATCH thành công |
| `201 Created` | Tạo mới thành công | POST tạo tài nguyên mới (đăng ký, tạo chuyến, tạo thanh toán...) |
| `202 Accepted` | Đã nhận yêu cầu, xử lý bất đồng bộ | Các API internal xử lý nền (matching, notification) |
| `204 No Content` | Thành công, không có nội dung trả về | POST không cần trả dữ liệu (vd. gửi vị trí) |
| `400 Bad Request` | Dữ liệu đầu vào không hợp lệ | Thiếu trường bắt buộc, sai định dạng |
| `401 Unauthorized` | Chưa xác thực / token hết hạn / sai thông tin đăng nhập | Thiếu hoặc sai `Authorization` header |
| `403 Forbidden` | Đã xác thực nhưng không đủ quyền | Gọi API admin bằng tài khoản không phải operator |
| `404 Not Found` | Không tìm thấy tài nguyên | ID không tồn tại |
| `409 Conflict` | Xung đột trạng thái nghiệp vụ | Hủy chuyến đã hoàn thành, đánh giá 2 lần, chuyển trạng thái sai thứ tự... |
| `500 Internal Server Error` | Lỗi hệ thống | Lỗi không lường trước |

### 4.2. Quy ước đặt `error_code` (lỗi nghiệp vụ)

Mọi response lỗi (4xx) đều trả về body theo format:

```json
{
  "error_code": "RESOURCE_REASON",
  "message": "Mô tả lỗi dễ hiểu cho người dùng cuối"
}
```

Quy ước đặt tên `error_code`: `<TÊN_TÀI_NGUYÊN>_<LÝ_DO>`, viết hoa, nối bằng `_`. Ví dụ:

| error_code | Ý nghĩa |
|---|---|
| `TRIP_ALREADY_ACTIVE` | Khách hàng đang có chuyến chưa hoàn thành (không cho tạo chuyến mới) |
| `TRIP_CANCEL_NOT_ALLOWED` | Chuyến đã ở trạng thái không cho phép hủy |
| `TRIP_STATUS_INVALID_TRANSITION` | Chuyển trạng thái chuyến sai thứ tự (vi phạm QT-09) |
| `TRIP_ALREADY_RATED` | Chuyến đã được đánh giá trước đó |
| `DRIVER_PROFILE_INCOMPLETE` | Hồ sơ/phương tiện tài xế chưa đầy đủ để bật sẵn sàng (QT-07) |
| `PAYMENT_RETRY_NOT_ALLOWED` | Giao dịch chưa ở trạng thái failed, không thể thử lại |
| `WEBHOOK_SIGNATURE_INVALID` | Chữ ký webhook từ cổng thanh toán không hợp lệ |
| `USER_NOT_FOUND` | Không tìm thấy người dùng |

*(Danh sách đầy đủ mọi error_code cho từng endpoint xem trực tiếp trong `openapi.yaml`, mục `responses` của mỗi operation.)*

---

## 5. Cách xem chi tiết từng API

File `openapi.yaml` chứa đầy đủ 9 phần cho mỗi API: Title, Endpoint, Method, URL Parameters, Message Payload, Header Parameters, Response Code, Error Codes, và có thể sinh Sample Calls tự động.

**Cách xem trực quan:**

1. **Swagger Editor** (khuyến nghị): vào https://editor.swagger.io → File → Import file → chọn `openapi.yaml` → xem giao diện trực quan từng endpoint, có thể "Try it out" để test trực tiếp
2. **Postman**: File → Import → chọn `openapi.yaml` → Postman tự tạo Collection với đầy đủ request mẫu cho từng endpoint
3. **VS Code**: cài extension "OpenAPI (Swagger) Editor" để xem preview ngay trong editor

---

## 6. Các FR không có endpoint riêng

3 FR sau đây **không sinh ra API riêng** vì bản chất là ràng buộc/yêu cầu kiến trúc, không phải hành vi có thể "gọi" được (xem lại phân tích Cách A khi thiết kế API):

| FR | Nội dung | Vì sao không có endpoint | Được hiện thực ở đâu |
|---|---|---|---|
| FR-06 | Xác thực & phân quyền theo vai trò | Là cross-cutting concern, áp dụng lên **mọi** API khác dưới dạng middleware | Middleware xác thực JWT + kiểm tra role, chạy trước mọi request (trừ register/login) |
| FR-26 | Không lưu thông tin nhạy cảm của thẻ/tài khoản thanh toán | Là ràng buộc thiết kế (constraint), không phải hành động | Áp dụng trong toàn bộ `payment-service`: không có field nào chứa số thẻ trong schema `Payment` |
| FR-31 | Kiến trúc thông báo cho phép mở rộng kênh gửi mới | Là yêu cầu kiến trúc (adapter pattern), không phải 1 lời gọi API | Trường `channel` trong `POST /internal/notifications` để dạng string mở, xử lý qua adapter bên trong Notification Service |

---

## 7. Truy vết về SRS

Toàn bộ 32 API trên được thiết kế dựa trên bộ Functional Requirements (FR) đã phân tích trong `srs.md`:

- Xem **Bước 6 – Functional Requirements** để biết chi tiết từng FR trước khi được chuyển thành API
- Xem **Bước 7 – Business Rules** để hiểu các ràng buộc nghiệp vụ (QT-xx) được nhắc đến trong `description` của từng API
- Xem **Bước 14 – Traceability Matrix** để đối chiếu ngược từ BR → FR → UC → AC, đảm bảo không có yêu cầu nào bị bỏ sót khi thiết kế API

**Lưu ý về số lượng file:** Để đáp ứng yêu cầu tối thiểu 10 file `.yaml`, các API được tách thành 11 file theo resource (thay vì gộp cứng theo 7 service như phân tích ban đầu ở Bước 6). File `openapi.yaml` trong cùng thư mục là bản gộp của cả 11 file này thành 1 spec thống nhất, dùng để import vào Swagger/Postman.
