# CAB System – API Documentation

## 1. Giới thiệu

Thư mục `API` chứa tài liệu đặc tả API của **CAB System – Nền tảng đặt xe**.

CAB System là nền tảng đặt xe trực tuyến, hỗ trợ khách hàng tạo yêu cầu đặt xe, tìm và phân công tài xế, thực hiện chuyến đi, theo dõi trạng thái, tính cước, thanh toán và đánh giá tài xế.

Hệ thống được xây dựng theo định hướng **Service-Oriented Architecture (SOA) / Microservices**, trong đó các chức năng nghiệp vụ được tổ chức thành các nhóm API độc lập. Cách tổ chức này hỗ trợ khả năng mở rộng, triển khai từng phần và hạn chế ảnh hưởng khi một thành phần gặp sự cố.

Các API trong thư mục được mô tả bằng **OpenAPI 3.0.3 (YAML)** và được xây dựng dựa trên các yêu cầu, Use Case, quy trình nghiệp vụ và quy tắc nghiệp vụ trong tài liệu `srs.md`.

---

## 2. Cấu trúc tài liệu API

Các file YAML được phân chia theo từng nhóm nghiệp vụ chính của hệ thống:

| STT | File | Nhóm chức năng | Yêu cầu liên quan |
|---|---|---|---|
| 01 | `01_Authentication.yaml` | Xác thực và quản lý thông tin đăng nhập | FR-01.1, FR-01.2, FR-01.4 |
| 02 | `02_Customer.yaml` | Quản lý tài khoản khách hàng | FR-01.3 |
| 03 | `03_Driver.yaml` | Quản lý tài khoản, phương tiện và trạng thái tài xế | FR-02, FR-04.7 |
| 04 | `04_Ride_Trip.yaml` | Đặt xe và quản lý chuyến đi | FR-03 |
| 05 | `05_Matching.yaml` | Tìm kiếm và phân công tài xế | FR-04.1 – FR-04.6 |
| 06 | `06_Payment.yaml` | Tính cước và thanh toán | FR-05 |
| 07 | `07_Rating.yaml` | Đánh giá tài xế | FR-03.12 |
| 08 | `08_Notification.yaml` | Gửi thông báo | FR-06 |
| 09 | `09_Admin.yaml` | Quản trị và vận hành hệ thống | FR-07 |
| 10 | `10_Reporting.yaml` | Báo cáo hoạt động | FR-08 |

---

## 3. Cơ sở xây dựng API

Tài liệu API được xây dựng dựa trên các thành phần trong SRS:

```text
Business Requirements
        ↓
Functional Requirements
        ↓
Use Case Specification
        ↓
Business Process
        ↓
Business Rules
        ↓
API Specification
````

Các nhóm Functional Requirements được ánh xạ với API như sau:

| Nhóm  | Nội dung                | Functional Requirements                                         |
| ----- | ----------------------- | --------------------------------------------------------------- |
| FR-01 | Tài khoản khách hàng    | Đăng ký, đăng nhập, cập nhật và khôi phục tài khoản             |
| FR-02 | Tài khoản tài xế        | Đăng ký, quản lý thông tin tài xế, phương tiện và trạng thái    |
| FR-03 | Đặt xe và chuyến đi     | Tạo, theo dõi, cập nhật, hủy và xem lịch sử chuyến              |
| FR-04 | Tìm và phân công tài xế | Tìm tài xế, ưu tiên, gửi yêu cầu và xử lý từ chối/timeout       |
| FR-05 | Tính cước và thanh toán | Tính cước, thanh toán tiền mặt/điện tử và xử lý kết quả         |
| FR-06 | Thông báo               | Thông báo cho khách hàng và tài xế                              |
| FR-07 | Quản trị và vận hành    | Quản lý người dùng, tài xế, phương tiện, chuyến đi và giao dịch |
| FR-08 | Báo cáo                 | Báo cáo chuyến đi, doanh thu và hiệu suất tài xế                |

---

## 4. Luồng nghiệp vụ chính

Luồng nghiệp vụ chính của CAB System được thể hiện như sau:

```text
Khách hàng
    ↓
Tạo yêu cầu đặt xe
    ↓
Kiểm tra thông tin
    ↓
Tìm tài xế phù hợp
    ↓
Phân công tài xế
    ↓
Tài xế nhận chuyến
    ↓
Tài xế đến điểm đón
    ↓
Đón khách và bắt đầu chuyến
    ↓
Thực hiện chuyến đi
    ↓
Hoàn thành chuyến
    ↓
Tính cước
    ↓
Thanh toán
    ↓
Gửi thông báo kết quả
    ↓
Khách hàng đánh giá tài xế
```

Trong trường hợp tài xế từ chối hoặc không phản hồi, hệ thống tiếp tục tìm và phân công tài xế phù hợp khác.

Nếu không tìm được tài xế phù hợp, hệ thống thông báo cho khách hàng.

---

## 5. Xác thực và phân quyền

Các API yêu cầu xác thực sử dụng Bearer Token:

```http
Authorization: Bearer <access_token>
```

Các vai trò chính của hệ thống gồm:

* `Customer`: khách hàng
* `Driver`: tài xế
* `Operator/Admin`: nhân viên vận hành và quản trị

Việc kiểm tra quyền truy cập được thực hiện dựa trên vai trò của người dùng đối với từng chức năng.

Cơ chế **Bearer Token** là lựa chọn thiết kế cho tài liệu API. SRS yêu cầu hệ thống phải hỗ trợ xác thực và phân quyền nhưng không quy định cụ thể công nghệ hoặc cơ chế token nào.

---

## 6. Đặt xe và quản lý chuyến đi

Nhóm API `04_Ride_Trip.yaml` hỗ trợ các chức năng chính:

* Tạo yêu cầu đặt xe.
* Xem lịch sử chuyến đi.
* Xem thông tin chuyến đi.
* Theo dõi trạng thái chuyến.
* Hiển thị thông tin tài xế được phân công.
* Hiển thị vị trí tài xế và thời gian dự kiến đến.
* Hủy yêu cầu/chuyến đi.
* Cập nhật các trạng thái trong quá trình thực hiện chuyến.

Trạng thái chuyến được quản lý theo trình tự nghiệp vụ của hệ thống, từ khi tạo yêu cầu cho đến khi chuyến hoàn thành.

---

## 7. Tìm và phân công tài xế

Nhóm API `05_Matching.yaml` hỗ trợ quá trình tìm kiếm và phân công tài xế.

Hệ thống thực hiện:

1. Xác định các tài xế phù hợp.
2. Xem xét trạng thái sẵn sàng và vị trí.
3. Ưu tiên tài xế phù hợp.
4. Gửi yêu cầu nhận chuyến.
5. Xử lý trường hợp tài xế từ chối.
6. Xử lý trường hợp tài xế không phản hồi.
7. Tiếp tục tìm tài xế khác khi cần thiết.
8. Thông báo cho khách hàng nếu không tìm được tài xế.

Các tiêu chí ưu tiên cụ thể và thời gian phản hồi của tài xế chưa được xác định trong SRS nên không được cố định trong tài liệu API.

---

## 8. Tính cước và thanh toán

Nhóm API `06_Payment.yaml` hỗ trợ:

* Tính cước chuyến đi.
* Thanh toán bằng tiền mặt.
* Thanh toán điện tử.
* Gửi yêu cầu thanh toán đến Payment Gateway.
* Nhận kết quả thanh toán thông qua callback/webhook.
* Xử lý trường hợp thanh toán thất bại.
* Tra cứu lịch sử giao dịch.

Đối với thanh toán điện tử, hệ thống sử dụng **Payment Gateway** bên ngoài.

CAB System **không lưu trữ thông tin nhạy cảm của thẻ hoặc tài khoản ngân hàng**.

Công thức tính cước chi tiết chưa được xác định trong SRS. Vì vậy, API chỉ thể hiện chức năng tính cước dựa trên thông tin dịch vụ và chuyến đi, không tự quy định một công thức hoặc mức giá cụ thể.

---

## 9. Gửi thông báo

Nhóm API `08_Notification.yaml` hỗ trợ gửi thông báo trong các thời điểm quan trọng của quy trình:

### Đối với khách hàng

* Yêu cầu đặt xe được tiếp nhận.
* Tài xế được phân công.
* Tài xế đến điểm đón.
* Chuyến đi hoàn thành.
* Thanh toán thành công hoặc thất bại.

### Đối với tài xế

* Có chuyến đi phù hợp.
* Thông tin chuyến đi thay đổi.

Hệ thống được thiết kế theo hướng có thể mở rộng thêm các kênh thông báo trong tương lai.

Các kênh thông báo chính thức chưa được xác định đầy đủ trong SRS nên tài liệu API không cố định danh sách kênh ngoài phạm vi đã thống nhất.

---

## 10. Quản trị và vận hành

Nhóm API `09_Admin.yaml` hỗ trợ Operator/Admin trong việc:

* Quản lý khách hàng.
* Quản lý tài xế.
* Phê duyệt tài khoản tài xế.
* Quản lý phương tiện.
* Theo dõi các chuyến đi đang hoạt động.
* Theo dõi trạng thái tài xế.
* Xử lý các chuyến đi gặp sự cố.
* Tra cứu giao dịch.
* Thực hiện các chức năng theo quyền được cấp.

Các chức năng quản trị phải được kiểm soát bằng cơ chế phân quyền theo vai trò.

---

## 11. Báo cáo

Nhóm API `10_Reporting.yaml` cung cấp các báo cáo phục vụ hoạt động quản lý và vận hành:

* Số lượng chuyến đi theo ngày/tuần/tháng.
* Doanh thu.
* Tỷ lệ hoàn thành chuyến.
* Tỷ lệ hủy chuyến.
* Hiệu suất tài xế.

Các báo cáo hỗ trợ Operator/Admin theo dõi tình hình hoạt động của hệ thống và phục vụ công tác quản lý.

---

## 12. Bảo mật dữ liệu

CAB System có yêu cầu bảo vệ các loại dữ liệu:

* Thông tin cá nhân khách hàng.
* Thông tin tài xế.
* Thông tin phương tiện.
* Thông tin vị trí.
* Thông tin giao dịch.

Các API cần tuân thủ cơ chế xác thực, phân quyền và kiểm soát truy cập phù hợp.

Các thao tác quan trọng của hệ thống cần được ghi nhận Audit Log theo yêu cầu trong SRS.

Thời gian lưu trữ Audit Log và dữ liệu vị trí chưa được xác định cụ thể trong SRS.

---

## 13. Các quy tắc nghiệp vụ chưa được xác định

Một số quy tắc nghiệp vụ vẫn đang ở trạng thái chưa xác định trong SRS. Do đó, các API không tự đưa ra giá trị cố định cho các nội dung này.

Các nội dung gồm:

* Công thức tính cước chi tiết.
* Tiêu chí ưu tiên tài xế.
* Thời gian tài xế phản hồi.
* Chính sách hủy chuyến.
* Cách xử lý khi mất kết nối mạng.
* Chính sách thử lại khi thanh toán thất bại.
* Thời gian lưu Audit Log.
* Thời gian lưu dữ liệu vị trí.
* Các kênh thông báo chính thức.

Khi các quy tắc nghiệp vụ được xác định, các API và schema liên quan cần được cập nhật tương ứng.

---

## 14. Chuẩn API

Các file trong thư mục sử dụng:

* **OpenAPI Specification 3.0.3**
* Định dạng **YAML**
* Dữ liệu request/response chủ yếu sử dụng **JSON**
* HTTP Methods: `GET`, `POST`, `PUT`, `PATCH`, `DELETE`
* HTTP Status Codes để biểu diễn kết quả xử lý.

Các file có thể được kiểm tra và hiển thị bằng các công cụ hỗ trợ OpenAPI như **Swagger Editor**, **Swagger UI** hoặc **Postman**.

---

## 15. Quy ước thiết kế API

### HTTP Method

| Method   | Mục đích                             |
| -------- | ------------------------------------ |
| `GET`    | Lấy dữ liệu                          |
| `POST`   | Tạo mới hoặc thực hiện một hành động |
| `PUT`    | Cập nhật toàn bộ thông tin           |
| `PATCH`  | Cập nhật một phần thông tin          |
| `DELETE` | Xóa hoặc hủy dữ liệu theo nghiệp vụ  |

### Response

API sử dụng HTTP Status Code để biểu diễn kết quả xử lý, ví dụ:

```text
200 OK
201 Created
400 Bad Request
401 Unauthorized
403 Forbidden
404 Not Found
409 Conflict
500 Internal Server Error
```

Các mã lỗi cụ thể được mô tả trong từng file API khi cần thiết.

---

## 16. Quan hệ giữa API và SRS

Mỗi nhóm API được xây dựng từ các yêu cầu chức năng tương ứng trong SRS.

Ví dụ:

```text
FR-03.1
  ↓
UC-01 – Đặt xe
  ↓
Business Process – Tạo yêu cầu đặt xe
  ↓
04_Ride_Trip.yaml
  ↓
POST /rides
```

Tương tự:

```text
FR-04.1 – FR-04.6
  ↓
UC-02 – Tìm & phân công tài xế
  ↓
Business Process – Tìm và phân công tài xế
  ↓
05_Matching.yaml
```

Việc tổ chức API theo nhóm giúp duy trì khả năng truy vết giữa yêu cầu nghiệp vụ và đặc tả API.

---

## 17. Lưu ý về phạm vi thiết kế

Các endpoint, request/response schema và cơ chế Bearer Token trong tài liệu này là **thiết kế API được xây dựng dựa trên SRS**.

SRS không quy định nguyên văn tất cả tên endpoint, cấu trúc JSON hoặc HTTP Method. Vì vậy, những thành phần này được lựa chọn nhằm biểu diễn các yêu cầu chức năng của hệ thống dưới dạng API.

Các thiết kế này cần được cập nhật nếu yêu cầu nghiệp vụ chính thức của CAB System thay đổi.

---

## 18. Danh sách Use Case liên quan

Các API trong thư mục hỗ trợ các Use Case chính:

| Mã    | Use Case                     |
| ----- | ---------------------------- |
| UC-01 | Đặt xe                       |
| UC-02 | Tìm & phân công tài xế       |
| UC-03 | Theo dõi trạng thái chuyến   |
| UC-04 | Cập nhật trạng thái chuyến   |
| UC-05 | Thanh toán                   |
| UC-06 | Đánh giá tài xế              |
| UC-07 | Gửi thông báo                |
| UC-08 | Quản lý tài khoản khách hàng |
| UC-09 | Quản lý tài khoản tài xế     |
| UC-10 | Quản trị & vận hành          |
| UC-11 | Xem báo cáo                  |

---

## 19. Kết luận

Thư mục `API` cung cấp đặc tả API cho các chức năng chính của CAB System, bao gồm quản lý tài khoản, đặt xe, quản lý chuyến đi, tìm và phân công tài xế, thanh toán, thông báo, đánh giá, quản trị và báo cáo.

Các API được tổ chức theo từng nhóm nghiệp vụ và có khả năng mở rộng phù hợp với định hướng **SOA / Microservices** của hệ thống.

Tài liệu API cần được duy trì đồng bộ với `srs.md` trong suốt quá trình phát triển dự án.
