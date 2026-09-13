# Payment API

## 1. Lấy thông tin cước
**GET `/rides/{rideId}/fare`**

Response:
```json
{"rideId":"RIDE001","amount":85000,"currency":"VND"}
```

> Công thức tính cước chi tiết chưa được xác định trong SRS.

## 2. Thanh toán
**POST `/rides/{rideId}/payments`**

Tiền mặt:
```json
{"paymentMethod":"CASH"}
```

Điện tử:
```json
{"paymentMethod":"ELECTRONIC"}
```

Response:
```json
{"paymentId":"PAY001","rideId":"RIDE001","amount":85000,"paymentMethod":"CASH","status":"PAID"}
```

## 3. Xem thanh toán
**GET `/rides/{rideId}/payment`**

## 4. Payment Gateway Callback
**POST `/payments/callback`**

Request:
```json
{"paymentId":"PAY001","transactionId":"TXN001","status":"SUCCESS"}
```

CAB System không lưu thông tin nhạy cảm của thẻ hoặc tài khoản ngân hàng.
