# CAB System API Document

## Danh sách file

| File | Nội dung |
|---|---|
| `01_API_Overview.md` | Tổng quan, Base URL, Authentication |
| `02_Authentication_API.md` | Đăng ký, đăng nhập, mật khẩu |
| `03_Customer_API.md` | Tài khoản khách hàng |
| `04_Driver_API.md` | Tài khoản, phương tiện, trạng thái, vị trí tài xế |
| `05_Ride_Trip_API.md` | Đặt và quản lý chuyến |
| `06_Matching_API.md` | Tìm và phân công tài xế |
| `07_Payment_API.md` | Cước, thanh toán, Payment Gateway |
| `08_Rating_API.md` | Đánh giá tài xế |
| `09_Notification_API.md` | Thông báo |
| `10_Admin_Reporting_API.md` | Quản trị và báo cáo |

## Luồng chính

```text
Đặt xe → Tìm & phân công tài xế → Thực hiện chuyến
→ Theo dõi → Hoàn thành → Tính cước → Thanh toán
→ Thông báo → Đánh giá tài xế
```
