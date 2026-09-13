# CAB System API Specification

## 1. Tổng quan
CAB System là nền tảng đặt xe trực tuyến, hỗ trợ quản lý tài khoản, đặt xe, tìm và phân công tài xế, theo dõi chuyến, tính cước và thanh toán, thông báo, đánh giá, quản trị và báo cáo.

## 2. Base URL
```text
http://localhost:8080/api/v1
```

## 3. Authentication
Các API yêu cầu xác thực sử dụng Bearer Token.
```http
Authorization: Bearer <access_token>
```
Các API đăng ký và đăng nhập không yêu cầu access token.

## 4. HTTP Response Codes
| Code | Ý nghĩa |
|---|---|
| 200 | Request thành công |
| 201 | Tạo tài nguyên thành công |
| 400 | Request không hợp lệ |
| 401 | Chưa xác thực |
| 403 | Không có quyền |
| 404 | Không tìm thấy tài nguyên |
| 409 | Xung đột dữ liệu/trạng thái |
| 500 | Lỗi máy chủ |

## 5. Error Response
```json
{"code":"ERROR_CODE","message":"Mô tả lỗi"}
```

> Công thức tính cước, tiêu chí ưu tiên tài xế, timeout và chính sách hủy chuyến chưa được xác định trong SRS.
