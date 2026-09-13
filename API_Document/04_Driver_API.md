# Driver API

## 1. Đăng ký tài khoản tài xế
**POST `/drivers/register`**

Request:
```json
{"fullName":"Tran Van B","phone":"0912345678","email":"driver@example.com","password":"12345678","licenseNumber":"79A123456"}
```

Response `201 Created`:
```json
{"driverId":"DRV001","status":"PENDING_APPROVAL","message":"Đăng ký tài khoản tài xế thành công"}
```

## 2. Xem thông tin tài xế
**GET `/drivers/me`**

## 3. Cập nhật thông tin tài xế
**PUT `/drivers/me`**

Request:
```json
{"fullName":"Tran Van B","email":"driver@example.com","licenseNumber":"79A123456"}
```

## 4. Cập nhật phương tiện
**PUT `/drivers/me/vehicle`**

Request:
```json
{"vehicleType":"MOTORBIKE","licensePlate":"59A12345","vehicleModel":"Honda Vision"}
```

## 5. Cập nhật trạng thái
**PUT `/drivers/me/status`**

Request:
```json
{"status":"ONLINE"}
```

## 6. Cập nhật vị trí
**PUT `/drivers/me/location`**

Request:
```json
{"latitude":10.762622,"longitude":106.660172}
```
