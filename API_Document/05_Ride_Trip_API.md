# Ride & Trip API

## 1. Đặt xe
**POST `/rides`**

Request:
```json
{"pickupLocation":{"latitude":10.762622,"longitude":106.660172,"address":"Điểm đón"},"dropoffLocation":{"latitude":10.776530,"longitude":106.700981,"address":"Điểm trả"},"vehicleType":"MOTORBIKE"}
```

Response `201 Created`:
```json
{"rideId":"RIDE001","status":"SEARCHING_DRIVER","message":"Đã tạo yêu cầu đặt xe"}
```

## 2. Xem lịch sử chuyến
**GET `/rides/history`**

## 3. Xem thông tin/trạng thái chuyến
**GET `/rides/{rideId}`**

## 4. Hủy chuyến
**POST `/rides/{rideId}/cancel`**

Request:
```json
{"reason":"Không còn nhu cầu"}
```

## 5. Cập nhật trạng thái chuyến
**PUT `/rides/{rideId}/status`**

Request:
```json
{"status":"IN_TRANSIT"}
```

Trạng thái chính:
`SEARCHING_DRIVER`, `DRIVER_ASSIGNED`, `DRIVER_ARRIVED`, `PICKED_UP`, `IN_TRANSIT`, `COMPLETED`, `CANCELLED`.

> Chính sách hủy chuyến chưa được xác định trong SRS.
