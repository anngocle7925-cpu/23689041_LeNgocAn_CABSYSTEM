# Admin & Reporting API

Các API quản trị yêu cầu quyền Operator/Admin phù hợp.

## 1. Khách hàng
**GET `/admin/customers`**

## 2. Tài xế
**GET `/admin/drivers`**

## 3. Tạo tài khoản tài xế
**POST `/admin/drivers`**

## 4. Duyệt tài khoản tài xế
**PUT `/admin/drivers/{driverId}/approval`**

Request:
```json
{"status":"APPROVED"}
```

## 5. Phương tiện
**GET `/admin/vehicles`**

## 6. Chuyến đang hoạt động
**GET `/admin/rides/active`**

## 7. Phân công lại tài xế
**POST `/admin/rides/{rideId}/reassign`**

Request:
```json
{"driverId":"DRV002"}
```

## 8. Tra cứu giao dịch
**GET `/admin/payments/{paymentId}`**

# Reporting

## 9. Số lượng chuyến
**GET `/admin/reports/trips`**

Query: `fromDate`, `toDate`

## 10. Doanh thu
**GET `/admin/reports/revenue`**

Query: `fromDate`, `toDate`

## 11. Tỷ lệ hoàn thành
**GET `/admin/reports/completion-rate`**

Query: `fromDate`, `toDate`

## 12. Tỷ lệ hủy
**GET `/admin/reports/cancellation-rate`**

Query: `fromDate`, `toDate`

## 13. Hiệu suất tài xế
**GET `/admin/reports/driver-performance`**

Query: `fromDate`, `toDate`
