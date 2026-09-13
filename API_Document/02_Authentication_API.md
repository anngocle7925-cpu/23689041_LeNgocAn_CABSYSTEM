# Authentication API

## 1. Đăng ký tài khoản
**POST `/auth/register`**

Request:
```json
{"phone":"0901234567","email":"customer@example.com","password":"12345678","fullName":"Nguyen Van A"}
```

Response `201 Created`:
```json
{"userId":"CUS001","message":"Đăng ký tài khoản thành công"}
```

## 2. Đăng nhập
**POST `/auth/login`**

Request:
```json
{"phone":"0901234567","password":"12345678"}
```

Response `200 OK`:
```json
{"accessToken":"access-token","userId":"CUS001","role":"CUSTOMER"}
```

## 3. Đổi mật khẩu
**POST `/auth/password/change`**

Header:
```http
Authorization: Bearer <access_token>
```

Request:
```json
{"currentPassword":"12345678","newPassword":"87654321"}
```

## 4. Khôi phục mật khẩu
**POST `/auth/password/recover`**

Request:
```json
{"phone":"0901234567"}
```
