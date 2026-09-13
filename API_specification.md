Được. Dưới đây là **bản hoàn chỉnh `API_specification.md`** để bạn **copy toàn bộ và dán đè vào file hiện tại**.

Mình giữ nội dung bám theo các đặc tả CAB System của bạn, đồng thời **không tự chốt các quy tắc nghiệp vụ mà SRS đang để chưa xác định** như công thức tính cước, thời gian timeout tài xế, tiêu chí ưu tiên tài xế, chính sách hủy chuyến.

````markdown
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

Các quy tắc nghiệp vụ chưa được xác định trong SRS, như công thức tính cước, tiêu chí ưu tiên tài xế, thời gian timeout và chính sách hủy chuyến, không được cố định trong API Document.

---

## 2. Base URL

```text
http://localhost:8080/api/v1
````

---

## 3. Authentication

Các API yêu cầu xác thực sử dụng Bearer Token.

### Header

```http
Authorization: Bearer <access_token>
```

Các API đăng ký và đăng nhập không yêu cầu access token.

---

# 4. Authentication API

## 4.1 Đăng ký tài khoản

### Endpoint

```http
POST /auth/register
```

### Mô tả

Cho phép khách hàng đăng ký tài khoản mới.

### Request Body

```json
{
  "phone": "0901234567",
  "email": "customer@example.com",
  "password": "12345678",
  "fullName": "Nguyen Van A"
}
```

### Response thành công

**HTTP 201 Created**

```json
{
  "userId": "CUS001",
  "message": "Đăng ký tài khoản thành công"
}
```

### Response lỗi

**HTTP 400 Bad Request**

```json
{
  "code": "INVALID_REQUEST",
  "message": "Thông tin đăng ký không hợp lệ"
}
```

---

## 4.2 Đăng nhập

### Endpoint

```http
POST /auth/login
```

### Mô tả

Cho phép khách hàng hoặc tài xế đăng nhập vào hệ thống.

### Request Body

```json
{
  "phone": "0901234567",
  "password": "12345678"
}
```

### Response thành công

**HTTP 200 OK**

```json
{
  "accessToken": "access-token",
  "userId": "CUS001",
  "role": "CUSTOMER"
}
```

### Response lỗi

**HTTP 401 Unauthorized**

```json
{
  "code": "INVALID_CREDENTIALS",
  "message": "Thông tin đăng nhập không chính xác"
}
```

---

## 4.3 Đổi mật khẩu

### Endpoint

```http
POST /auth/password/change
```

### Mô tả

Cho phép người dùng đã đăng nhập thay đổi mật khẩu.

### Authentication

```http
Authorization: Bearer <access_token>
```

### Request Body

```json
{
  "currentPassword": "12345678",
  "newPassword": "87654321"
}
```

### Response thành công

**HTTP 200 OK**

```json
{
  "message": "Đổi mật khẩu thành công"
}
```

### Response lỗi

**HTTP 400 Bad Request**

```json
{
  "code": "INVALID_PASSWORD",
  "message": "Mật khẩu hiện tại không chính xác"
}
```

---

## 4.4 Khôi phục mật khẩu

### Endpoint

```http
POST /auth/password/recover
```

### Mô tả

Cho phép người dùng thực hiện quy trình khôi phục mật khẩu.

### Request Body

```json
{
  "phone": "0901234567"
}
```

### Response thành công

**HTTP 200 OK**

```json
{
  "message": "Yêu cầu khôi phục mật khẩu đã được tiếp nhận"
}
```

---

# 5. Customer API

## 5.1 Xem thông tin tài khoản

### Endpoint

```http
GET /customers/me
```

### Authentication

```http
Authorization: Bearer <access_token>
```

### Mô tả

Lấy thông tin tài khoản của khách hàng đang đăng nhập.

### Response thành công

**HTTP 200 OK**

```json
{
  "customerId": "CUS001",
  "fullName": "Nguyen Van A",
  "phone": "0901234567",
  "email": "customer@example.com"
}
```

---

## 5.2 Cập nhật thông tin tài khoản

### Endpoint

```http
PUT /customers/me
```

### Authentication

```http
Authorization: Bearer <access_token>
```

### Request Body

```json
{
  "fullName": "Nguyen Van B",
  "email": "customer@example.com"
}
```

### Response thành công

**HTTP 200 OK**

```json
{
  "message": "Cập nhật thông tin thành công"
}
```

### Response lỗi

**HTTP 400 Bad Request**

```json
{
  "code": "INVALID_REQUEST",
  "message": "Thông tin cập nhật không hợp lệ"
}
```

---

# 6. Driver API

## 6.1 Đăng ký tài khoản tài xế

### Endpoint

```http
POST /drivers/register
```

### Mô tả

Cho phép tài xế đăng ký tài khoản. Tài khoản đăng ký mới cần được Operator/Admin phê duyệt trước khi có thể nhận chuyến.

### Request Body

```json
{
  "fullName": "Tran Van B",
  "phone": "0912345678",
  "email": "driver@example.com",
  "password": "12345678",
  "licenseNumber": "79A123456"
}
```

### Response thành công

**HTTP 201 Created**

```json
{
  "driverId": "DRV001",
  "status": "PENDING_APPROVAL",
  "message": "Đăng ký tài khoản tài xế thành công"
}
```

---

## 6.2 Xem thông tin tài xế

### Endpoint

```http
GET /drivers/me
```

### Authentication

```http
Authorization: Bearer <access_token>
```

### Response thành công

**HTTP 200 OK**

```json
{
  "driverId": "DRV001",
  "fullName": "Tran Van B",
  "phone": "0912345678",
  "email": "driver@example.com",
  "status": "APPROVED"
}
```

---

## 6.3 Cập nhật thông tin tài xế

### Endpoint

```http
PUT /drivers/me
```

### Authentication

```http
Authorization: Bearer <access_token>
```

### Request Body

```json
{
  "fullName": "Tran Van B",
  "email": "driver@example.com",
  "licenseNumber": "79A123456"
}
```

### Response thành công

**HTTP 200 OK**

```json
{
  "message": "Cập nhật thông tin tài xế thành công"
}
```

---

## 6.4 Cập nhật thông tin phương tiện

### Endpoint

```http
PUT /drivers/me/vehicle
```

### Authentication

```http
Authorization: Bearer <access_token>
```

### Request Body

```json
{
  "vehicleType": "MOTORBIKE",
  "licensePlate": "59A12345",
  "vehicleModel": "Honda Vision"
}
```

### Response thành công

**HTTP 200 OK**

```json
{
  "message": "Cập nhật thông tin phương tiện thành công"
}
```

---

## 6.5 Cập nhật trạng thái sẵn sàng

### Endpoint

```http
PUT /drivers/me/status
```

### Authentication

```http
Authorization: Bearer <access_token>
```

### Request Body

```json
{
  "status": "ONLINE"
}
```

### Response thành công

**HTTP 200 OK**

```json
{
  "driverId": "DRV001",
  "status": "ONLINE"
}
```

---

## 6.6 Cập nhật vị trí tài xế

### Endpoint

```http
PUT /drivers/me/location
```

### Authentication

```http
Authorization: Bearer <access_token>
```

### Request Body

```json
{
  "latitude": 10.762622,
  "longitude": 106.660172
}
```

### Response thành công

**HTTP 200 OK**

```json
{
  "message": "Cập nhật vị trí thành công"
}
```

---

# 7. Ride API

## 7.1 Đặt xe

### Endpoint

```http
POST /rides
```

### Authentication

```http
Authorization: Bearer <access_token>
```

### Mô tả

Cho phép khách hàng tạo yêu cầu đặt xe.

### Request Body

```json
{
  "pickupLocation": {
    "latitude": 10.762622,
    "longitude": 106.660172,
    "address": "Trường Đại học Công nghiệp TP.HCM"
  },
  "dropoffLocation": {
    "latitude": 10.776530,
    "longitude": 106.700981,
    "address": "Chợ Bến Thành"
  },
  "vehicleType": "MOTORBIKE"
}
```

### Response thành công

**HTTP 201 Created**

```json
{
  "rideId": "RIDE001",
  "status": "SEARCHING_DRIVER",
  "message": "Đã tạo yêu cầu đặt xe"
}
```

### Response lỗi

**HTTP 400 Bad Request**

```json
{
  "code": "INVALID_RIDE_REQUEST",
  "message": "Thông tin đặt xe không hợp lệ"
}
```

---

## 7.2 Xem lịch sử chuyến

### Endpoint

```http
GET /rides/history
```

### Authentication

```http
Authorization: Bearer <access_token>
```

### Mô tả

Cho phép khách hàng xem lịch sử các chuyến đã thực hiện.

### Response thành công

**HTTP 200 OK**

```json
{
  "rides": [
    {
      "rideId": "RIDE001",
      "status": "COMPLETED",
      "amount": 85000
    }
  ]
}
```

---

## 7.3 Xem thông tin chuyến

### Endpoint

```http
GET /rides/{rideId}
```

### Authentication

```http
Authorization: Bearer <access_token>
```

### Path Parameter

| Parameter | Kiểu   | Mô tả     |
| --------- | ------ | --------- |
| rideId    | string | Mã chuyến |

### Response thành công

**HTTP 200 OK**

```json
{
  "rideId": "RIDE001",
  "status": "IN_TRANSIT",
  "driver": {
    "driverId": "DRV001",
    "name": "Tran Van B"
  },
  "pickupLocation": {
    "latitude": 10.762622,
    "longitude": 106.660172
  },
  "dropoffLocation": {
    "latitude": 10.776530,
    "longitude": 106.700981
  }
}
```

---

## 7.4 Hủy chuyến

### Endpoint

```http
POST /rides/{rideId}/cancel
```

### Authentication

```http
Authorization: Bearer <access_token>
```

### Request Body

```json
{
  "reason": "Không còn nhu cầu"
}
```

### Response thành công

**HTTP 200 OK**

```json
{
  "rideId": "RIDE001",
  "status": "CANCELLED",
  "message": "Hủy chuyến thành công"
}
```

> Chính sách và điều kiện hủy chuyến chưa được xác định trong SRS.

---

# 8. Matching API

## 8.1 Tìm và phân công tài xế

### Endpoint

```http
POST /rides/{rideId}/matching
```

### Authentication

```http
Authorization: Bearer <access_token>
```

### Mô tả

Hệ thống tìm và phân công tài xế phù hợp dựa trên vị trí, trạng thái sẵn sàng và các tiêu chí nghiệp vụ.

### Response thành công

**HTTP 200 OK**

```json
{
  "rideId": "RIDE001",
  "driverId": "DRV001",
  "status": "DRIVER_ASSIGNED"
}
```

### Trường hợp không tìm được tài xế

**HTTP 200 OK**

```json
{
  "rideId": "RIDE001",
  "status": "NO_DRIVER_AVAILABLE",
  "message": "Không tìm thấy tài xế phù hợp"
}
```

> Tiêu chí ưu tiên tài xế, bán kính tìm kiếm và thời gian timeout chưa được xác định trong SRS.

---

## 8.2 Tài xế chấp nhận chuyến

### Endpoint

```http
POST /rides/{rideId}/accept
```

### Authentication

```http
Authorization: Bearer <access_token>
```

### Response thành công

**HTTP 200 OK**

```json
{
  "rideId": "RIDE001",
  "status": "DRIVER_ACCEPTED"
}
```

---

## 8.3 Tài xế từ chối chuyến

### Endpoint

```http
POST /rides/{rideId}/reject
```

### Authentication

```http
Authorization: Bearer <access_token>
```

### Request Body

```json
{
  "reason": "Không thể nhận chuyến"
}
```

### Response thành công

**HTTP 200 OK**

```json
{
  "rideId": "RIDE001",
  "status": "SEARCHING_DRIVER"
}
```

Sau khi tài xế từ chối hoặc không phản hồi, hệ thống có thể tiếp tục tìm tài xế khác theo quy tắc nghiệp vụ.

---

# 9. Trip API

## 9.1 Cập nhật trạng thái chuyến

### Endpoint

```http
PUT /rides/{rideId}/status
```

### Authentication

```http
Authorization: Bearer <access_token>
```

### Request Body

```json
{
  "status": "IN_TRANSIT"
}
```

### Các trạng thái chính

```text
SEARCHING_DRIVER
DRIVER_ASSIGNED
DRIVER_ARRIVED
PICKED_UP
IN_TRANSIT
COMPLETED
CANCELLED
```

### Response thành công

**HTTP 200 OK**

```json
{
  "rideId": "RIDE001",
  "status": "IN_TRANSIT",
  "message": "Cập nhật trạng thái chuyến thành công"
}
```

---

## 9.2 Theo dõi trạng thái chuyến

### Endpoint

```http
GET /rides/{rideId}
```

### Authentication

```http
Authorization: Bearer <access_token>
```

### Mô tả

Cho phép khách hàng theo dõi thông tin và trạng thái hiện tại của chuyến.

Thông tin có thể bao gồm:

* Trạng thái chuyến
* Thông tin tài xế
* Vị trí đón
* Vị trí trả
* Vị trí hiện tại của tài xế
* Thông tin liên quan đến chuyến

---

## 9.3 Cập nhật vị trí tài xế

### Endpoint

```http
PUT /drivers/me/location
```

### Authentication

```http
Authorization: Bearer <access_token>
```

### Request Body

```json
{
  "latitude": 10.762622,
  "longitude": 106.660172
}
```

### Response thành công

**HTTP 200 OK**

```json
{
  "message": "Cập nhật vị trí thành công"
}
```

---

# 10. Payment API

## 10.1 Lấy thông tin cước chuyến

### Endpoint

```http
GET /rides/{rideId}/fare
```

### Authentication

```http
Authorization: Bearer <access_token>
```

### Mô tả

Lấy kết quả tính cước của chuyến sau khi hệ thống xử lý thông tin chuyến.

### Response thành công

**HTTP 200 OK**

```json
{
  "rideId": "RIDE001",
  "amount": 85000,
  "currency": "VND"
}
```

> Công thức tính cước chi tiết chưa được xác định trong SRS.

---

## 10.2 Thanh toán chuyến đi

### Endpoint

```http
POST /rides/{rideId}/payments
```

### Authentication

```http
Authorization: Bearer <access_token>
```

### Mô tả

Cho phép khách hàng thực hiện thanh toán cho chuyến đi bằng tiền mặt hoặc thanh toán điện tử.

### Request Body - Thanh toán tiền mặt

```json
{
  "paymentMethod": "CASH"
}
```

### Request Body - Thanh toán điện tử

```json
{
  "paymentMethod": "ELECTRONIC"
}
```

### Response thành công

**HTTP 200 OK**

```json
{
  "paymentId": "PAY001",
  "rideId": "RIDE001",
  "amount": 85000,
  "paymentMethod": "CASH",
  "status": "PAID"
}
```

### Response lỗi

**HTTP 400 Bad Request**

```json
{
  "code": "PAYMENT_FAILED",
  "message": "Thanh toán không thành công"
}
```

---

## 10.3 Xem thông tin thanh toán

### Endpoint

```http
GET /rides/{rideId}/payment
```

### Authentication

```http
Authorization: Bearer <access_token>
```

### Response thành công

**HTTP 200 OK**

```json
{
  "paymentId": "PAY001",
  "rideId": "RIDE001",
  "amount": 85000,
  "paymentMethod": "ELECTRONIC",
  "status": "PAID"
}
```

---

## 10.4 Payment Gateway Callback

### Endpoint

```http
POST /payments/callback
```

### Mô tả

Payment Gateway gửi kết quả giao dịch về CAB System thông qua callback/webhook.

### Request Body

```json
{
  "paymentId": "PAY001",
  "transactionId": "TXN001",
  "status": "SUCCESS"
}
```

### Response thành công

**HTTP 200 OK**

```json
{
  "message": "Đã tiếp nhận kết quả thanh toán"
}
```

CAB System không lưu thông tin nhạy cảm của thẻ hoặc tài khoản ngân hàng.

---

# 11. Rating API

## 11.1 Đánh giá tài xế

### Endpoint

```http
POST /rides/{rideId}/rating
```

### Authentication

```http
Authorization: Bearer <access_token>
```

### Mô tả

Cho phép khách hàng đánh giá tài xế sau khi chuyến đi hoàn thành.

### Request Body

```json
{
  "rating": 5,
  "comment": "Tài xế phục vụ tốt"
}
```

### Response thành công

**HTTP 201 Created**

```json
{
  "ratingId": "RATE001",
  "message": "Đánh giá tài xế thành công"
}
```

### Quy tắc

Chỉ chuyến đã hoàn thành mới được phép đánh giá.

---

# 12. Notification API

## 12.1 Gửi thông báo

### Endpoint

```http
POST /notifications
```

### Authentication

```http
Authorization: Bearer <access_token>
```

### Mô tả

Gửi thông báo đến khách hàng hoặc tài xế trong quá trình xử lý chuyến.

### Request Body

```json
{
  "userId": "CUS001",
  "type": "DRIVER_ASSIGNED",
  "message": "Tài xế đã được phân công cho chuyến đi của bạn"
}
```

### Response thành công

**HTTP 200 OK**

```json
{
  "notificationId": "NOTI001",
  "status": "SENT"
}
```

---

## 12.2 Xem thông báo

### Endpoint

```http
GET /notifications
```

### Authentication

```http
Authorization: Bearer <access_token>
```

### Mô tả

Cho phép người dùng xem các thông báo của mình.

### Response thành công

**HTTP 200 OK**

```json
{
  "notifications": [
    {
      "notificationId": "NOTI001",
      "type": "DRIVER_ASSIGNED",
      "message": "Tài xế đã được phân công",
      "status": "SENT"
    }
  ]
}
```

Hệ thống được thiết kế để có thể mở rộng thêm các kênh thông báo như Push Notification, SMS hoặc Email.

---

# 13. Admin API

Các API trong phần này yêu cầu tài khoản có quyền quản trị hoặc vận hành phù hợp.

## 13.1 Xem danh sách khách hàng

### Endpoint

```http
GET /admin/customers
```

### Authentication

```http
Authorization: Bearer <access_token>
```

### Response thành công

**HTTP 200 OK**

```json
{
  "customers": [
    {
      "customerId": "CUS001",
      "fullName": "Nguyen Van A",
      "phone": "0901234567"
    }
  ]
}
```

---

## 13.2 Xem danh sách tài xế

### Endpoint

```http
GET /admin/drivers
```

### Authentication

```http
Authorization: Bearer <access_token>
```

### Response thành công

**HTTP 200 OK**

```json
{
  "drivers": [
    {
      "driverId": "DRV001",
      "fullName": "Tran Van B",
      "status": "ONLINE"
    }
  ]
}
```

---

## 13.3 Tạo tài khoản tài xế

### Endpoint

```http
POST /admin/drivers
```

### Authentication

```http
Authorization: Bearer <access_token>
```

### Mô tả

Cho phép Operator/Admin tạo tài khoản tài xế.

### Request Body

```json
{
  "fullName": "Tran Van B",
  "phone": "0912345678",
  "email": "driver@example.com"
}
```

### Response thành công

**HTTP 201 Created**

```json
{
  "driverId": "DRV001",
  "message": "Tạo tài khoản tài xế thành công"
}
```

---

## 13.4 Duyệt tài khoản tài xế

### Endpoint

```http
PUT /admin/drivers/{driverId}/approval
```

### Authentication

```http
Authorization: Bearer <access_token>
```

### Request Body

```json
{
  "status": "APPROVED"
}
```

### Response thành công

**HTTP 200 OK**

```json
{
  "driverId": "DRV001",
  "status": "APPROVED",
  "message": "Duyệt tài khoản tài xế thành công"
}
```

---

## 13.5 Quản lý phương tiện

### Endpoint

```http
GET /admin/vehicles
```

### Authentication

```http
Authorization: Bearer <access_token>
```

### Response thành công

**HTTP 200 OK**

```json
{
  "vehicles": [
    {
      "vehicleId": "VEH001",
      "driverId": "DRV001",
      "vehicleType": "MOTORBIKE",
      "licensePlate": "59A12345"
    }
  ]
}
```

---

## 13.6 Theo dõi các chuyến đang hoạt động

### Endpoint

```http
GET /admin/rides/active
```

### Authentication

```http
Authorization: Bearer <access_token>
```

### Response thành công

**HTTP 200 OK**

```json
{
  "rides": [
    {
      "rideId": "RIDE001",
      "status": "IN_TRANSIT",
      "driverId": "DRV001"
    }
  ]
}
```

---

## 13.7 Phân công lại tài xế

### Endpoint

```http
POST /admin/rides/{rideId}/reassign
```

### Authentication

```http
Authorization: Bearer <access_token>
```

### Request Body

```json
{
  "driverId": "DRV002"
}
```

### Response thành công

**HTTP 200 OK**

```json
{
  "rideId": "RIDE001",
  "driverId": "DRV002",
  "message": "Phân công lại tài xế thành công"
}
```

---

## 13.8 Tra cứu giao dịch

### Endpoint

```http
GET /admin/payments/{paymentId}
```

### Authentication

```http
Authorization: Bearer <access_token>
```

### Response thành công

**HTTP 200 OK**

```json
{
  "paymentId": "PAY001",
  "rideId": "RIDE001",
  "amount": 85000,
  "paymentMethod": "ELECTRONIC",
  "status": "PAID"
}
```

---

# 14. Reporting API

Các API báo cáo yêu cầu tài khoản có quyền Operator/Admin phù hợp.

## 14.1 Báo cáo số lượng chuyến

### Endpoint

```http
GET /admin/reports/trips
```

### Query Parameters

| Parameter | Kiểu | Mô tả         |
| --------- | ---- | ------------- |
| fromDate  | date | Ngày bắt đầu  |
| toDate    | date | Ngày kết thúc |

### Ví dụ

```http
GET /admin/reports/trips?fromDate=2026-09-01&toDate=2026-09-13
```

### Response thành công

**HTTP 200 OK**

```json
{
  "reportType": "TRIPS",
  "fromDate": "2026-09-01",
  "toDate": "2026-09-13",
  "totalTrips": 1250
}
```

---

## 14.2 Báo cáo doanh thu

### Endpoint

```http
GET /admin/reports/revenue
```

### Query Parameters

```text
fromDate
toDate
```

### Response thành công

**HTTP 200 OK**

```json
{
  "reportType": "REVENUE",
  "fromDate": "2026-09-01",
  "toDate": "2026-09-13",
  "totalRevenue": 125000000
}
```

---

## 14.3 Tỷ lệ hoàn thành chuyến

### Endpoint

```http
GET /admin/reports/completion-rate
```

### Query Parameters

```text
fromDate
toDate
```

### Response thành công

**HTTP 200 OK**

```json
{
  "reportType": "COMPLETION_RATE",
  "completionRate": 92.5
}
```

---

## 14.4 Tỷ lệ hủy chuyến

### Endpoint

```http
GET /admin/reports/cancellation-rate
```

### Query Parameters

```text
fromDate
toDate
```

### Response thành công

**HTTP 200 OK**

```json
{
  "reportType": "CANCELLATION_RATE",
  "cancellationRate": 7.5
}
```

---

## 14.5 Hiệu suất tài xế

### Endpoint

```http
GET /admin/reports/driver-performance
```

### Query Parameters

```text
fromDate
toDate
```

### Response thành công

**HTTP 200 OK**

```json
{
  "reportType": "DRIVER_PERFORMANCE",
  "drivers": [
    {
      "driverId": "DRV001",
      "completedTrips": 120,
      "rating": 4.8
    }
  ]
}
```

---

# 15. HTTP Response Codes

Các mã HTTP chính được sử dụng:

| HTTP Code | Ý nghĩa                                            |
| --------- | -------------------------------------------------- |
| 200       | Request thành công                                 |
| 201       | Tạo tài nguyên thành công                          |
| 400       | Request không hợp lệ                               |
| 401       | Chưa xác thực hoặc thông tin xác thực không hợp lệ |
| 403       | Không có quyền truy cập                            |
| 404       | Không tìm thấy tài nguyên                          |
| 409       | Xung đột dữ liệu/trạng thái                        |
| 500       | Lỗi phía máy chủ                                   |

---

# 16. Error Response

Các API có thể trả về lỗi theo cấu trúc:

```json
{
  "code": "ERROR_CODE",
  "message": "Mô tả lỗi"
}
```

Ví dụ:

```json
{
  "code": "RIDE_NOT_FOUND",
  "message": "Không tìm thấy chuyến đi"
}
```

---

# 17. Quy tắc bảo mật API

* Các API yêu cầu xác thực phải gửi Bearer Token trong HTTP Header.
* Người dùng chỉ được truy cập các chức năng phù hợp với quyền của mình.
* Customer chỉ được quản lý thông tin và chuyến đi thuộc tài khoản của mình.
* Driver chỉ được thực hiện các chức năng thuộc tài khoản tài xế.
* Operator/Admin được truy cập các chức năng quản trị theo quyền được cấp.
* Thông tin cá nhân, thông tin phương tiện, vị trí và thông tin giao dịch phải được bảo vệ.
* CAB System không lưu thông tin nhạy cảm của thẻ hoặc tài khoản ngân hàng.
* Các thao tác quan trọng cần được ghi nhận vào audit log.

---

# 18. Luồng API chính của CAB System

Luồng nghiệp vụ chính:

```text
Customer
   |
   | POST /rides
   v
Tạo yêu cầu đặt xe
   |
   v
Tìm & phân công tài xế
   |
   +---- Tài xế từ chối/không phản hồi
   |             |
   |             v
   |       Tìm tài xế khác
   |
   v
Driver nhận chuyến
   |
   v
Cập nhật trạng thái chuyến
   |
   v
Theo dõi chuyến
   |
   v
Chuyến hoàn thành
   |
   v
Tính cước
   |
   v
Thanh toán
   |
   +---- Tiền mặt
   |
   +---- Thanh toán điện tử
                 |
                 v
          Payment Gateway
                 |
                 v
          Kết quả thanh toán
   |
   v
Gửi thông báo
   |
   v
Customer đánh giá tài xế
```

---

# 19. Các vấn đề nghiệp vụ chưa được xác định

Các nội dung sau chưa được cố định trong API Document vì chưa được xác định trong SRS:

1. Công thức tính cước chi tiết.
2. Tiêu chí ưu tiên tài xế.
3. Bán kính tìm kiếm tài xế.
4. Thời gian timeout khi tài xế không phản hồi.
5. Chính sách hủy chuyến.
6. Cách xử lý khi mất kết nối mạng.
7. Chính sách retry khi thanh toán điện tử thất bại.
8. Thời gian lưu trữ audit log.
9. Thời gian lưu trữ dữ liệu vị trí.
10. Các kênh thông báo được triển khai trong phiên bản đầu tiên.

Các nội dung trên sẽ được cập nhật khi có quy định nghiệp vụ chính thức.

---

# 20. Tổng kết API

| Nhóm           | API chính                                                        |
| -------------- | ---------------------------------------------------------------- |
| Authentication | Register, Login, Change Password, Recover Password               |
| Customer       | Xem/Cập nhật tài khoản                                           |
| Driver         | Đăng ký, xem/cập nhật tài khoản, phương tiện, trạng thái, vị trí |
| Ride           | Đặt xe, xem lịch sử, xem chuyến, hủy chuyến                      |
| Matching       | Tìm/phân công, nhận chuyến, từ chối chuyến                       |
| Trip           | Cập nhật trạng thái, theo dõi chuyến                             |
| Payment        | Tính cước, thanh toán, tra cứu thanh toán, callback              |
| Rating         | Đánh giá tài xế                                                  |
| Notification   | Gửi và xem thông báo                                             |
| Admin          | Quản lý khách hàng, tài xế, phương tiện, chuyến, giao dịch       |
| Reporting      | Chuyến, doanh thu, hoàn thành, hủy, hiệu suất tài xế             |


