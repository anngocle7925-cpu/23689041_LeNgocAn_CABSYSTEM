# Notification API

## 1. Gửi thông báo
**POST `/notifications`**

Request:
```json
{"userId":"CUS001","type":"DRIVER_ASSIGNED","message":"Tài xế đã được phân công cho chuyến đi của bạn"}
```

Response:
```json
{"notificationId":"NOTI001","status":"SENT"}
```

## 2. Xem thông báo
**GET `/notifications`**

Hệ thống có thể mở rộng thêm các kênh Push Notification, SMS hoặc Email.
