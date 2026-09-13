# Matching API

## 1. Tìm và phân công tài xế
**POST `/rides/{rideId}/matching`**

Mô tả: Hệ thống tìm và phân công tài xế phù hợp dựa trên vị trí, trạng thái sẵn sàng và các tiêu chí nghiệp vụ.

Response:
```json
{"rideId":"RIDE001","driverId":"DRV001","status":"DRIVER_ASSIGNED"}
```

Không tìm được tài xế:
```json
{"rideId":"RIDE001","status":"NO_DRIVER_AVAILABLE","message":"Không tìm thấy tài xế phù hợp"}
```

## 2. Tài xế chấp nhận chuyến
**POST `/rides/{rideId}/accept`**

Response:
```json
{"rideId":"RIDE001","status":"DRIVER_ACCEPTED"}
```

## 3. Tài xế từ chối chuyến
**POST `/rides/{rideId}/reject`**

Request:
```json
{"reason":"Không thể nhận chuyến"}
```

Response:
```json
{"rideId":"RIDE001","status":"SEARCHING_DRIVER"}
```

> Tiêu chí ưu tiên, bán kính tìm kiếm và thời gian timeout chưa được xác định trong SRS.
