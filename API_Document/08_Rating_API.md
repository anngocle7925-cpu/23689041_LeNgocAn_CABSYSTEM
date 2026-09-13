# Rating API

## Đánh giá tài xế
**POST `/rides/{rideId}/rating`**

Request:
```json
{"rating":5,"comment":"Tài xế phục vụ tốt"}
```

Response `201 Created`:
```json
{"ratingId":"RATE001","message":"Đánh giá tài xế thành công"}
```

Chỉ chuyến đã hoàn thành mới được phép đánh giá.
