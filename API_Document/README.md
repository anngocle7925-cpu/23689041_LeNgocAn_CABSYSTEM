# CAB System - OpenAPI YAML phân tách

Các file dưới đây là các tài liệu OpenAPI độc lập theo từng nhóm nghiệp vụ. Mỗi file có thể mở riêng trên Swagger Editor.

| File | Nhóm |
|---|---|
| `01_Authentication.yaml` | Authentication |
| `02_Customer.yaml` | Customer |
| `03_Driver.yaml` | Driver |
| `04_Ride_Trip.yaml` | Ride |
| `05_Matching.yaml` | Matching |
| `06_Payment.yaml` | Payment |
| `07_Rating.yaml` | Rating |
| `08_Notification.yaml` | Notification |
| `09_Admin.yaml` | Admin |
| `10_Reporting.yaml` | Reporting |

## Thứ tự nghiệp vụ chính

Authentication → Customer/Driver → Ride → Matching → Payment → Rating/Notification → Admin/Reporting.

> Các quy tắc chưa được chốt trong SRS như công thức tính cước, tiêu chí ưu tiên tài xế, timeout và chính sách hủy chuyến không được tự đặt giá trị cụ thể trong các file YAML.
