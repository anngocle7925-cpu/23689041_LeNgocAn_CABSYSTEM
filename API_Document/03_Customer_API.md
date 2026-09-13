# Customer API

## 1. Xem thông tin tài khoản
**GET `/customers/me`**

Header:
```http
Authorization: Bearer <access_token>
```

Response:
```json
{"customerId":"CUS001","fullName":"Nguyen Van A","phone":"0901234567","email":"customer@example.com"}
```

## 2. Cập nhật thông tin tài khoản
**PUT `/customers/me`**

Request:
```json
{"fullName":"Nguyen Van B","email":"customer@example.com"}
```

Response:
```json
{"message":"Cập nhật thông tin thành công"}
```
