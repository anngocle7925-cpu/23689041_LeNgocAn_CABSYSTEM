Bước 1:

Tìm hiểu nghiệp vụ, hệ thống hiện tại có những vấn đề gì, mục tiêu, vấn đề hiện tại là gì, ai là người sử dụng hệ thống 

### 1. Tổng quan nghiệp vụ & Vấn đề của hệ thống hiện tại

**Thực trạng hệ thống hiện tại:**
Doanh nghiệp hiện tại tiếp nhận nhu cầu đặt xe thông qua tổng đài hoặc một ứng dụng đơn giản.

**Các vấn đề tồn đọng:**

* **Phân công tài xế thủ công:** Việc gán chuyến chủ yếu làm bằng tay, tốn thời gian và không tối ưu theo vị trí.


* **Trải nghiệm khách hàng kém:** Khách hàng khó theo dõi trạng thái chuyến đi, tài xế đang ở đâu hay thời gian dự kiến đến (ETA).


* **Quản lý dữ liệu phân tán:** Thông tin thanh toán chưa được quản lý tập trung, gây khó khăn cho việc tra cứu và đối soát.


* **Khả năng mở rộng hạn chế:** Bộ phận vận hành gặp nhiều khó khăn khi số lượng người dùng tăng lên hoặc khi muốn mở rộng quy mô hệ thống.



---

### 2. Mục tiêu của hệ thống CAB mới

* **Tự động hóa quy trình:** Tự động hóa hoàn toàn luồng nghiệp vụ từ tạo chuyến, tìm/phân công tài xế theo vị trí thực tế, tính cước, thanh toán đến đánh giá sau chuyến.


* **Tối ưu khả năng mở rộng & Chịu tải:** Phục vụ số lượng lớn khách hàng và tài xế đồng thời; các thành phần có thể mở rộng độc lập khi tải tăng cao.


* **Cô lập sự cố (Fault Tolerance):** Đảm bảo tính sẵn sàng cao, lỗi ở dịch vụ thanh toán hoặc thông báo không làm ngưng trệ toàn bộ hệ thống đặt xe.


* **Linh hoạt cải tiến:** Kiến trúc đủ linh hoạt để bổ sung loại hình dịch vụ mới, phương thức thanh toán mới, nhà cung cấp thông báo mới mà không phải xây dựng lại toàn bộ ứng dụng.


* **Cam kết thời gian:** Phân tích, xây dựng và triển khai sản phẩm hoàn chỉnh trong vòng **7 tuần**.



---

### 3. Người sử dụng hệ thống (Tác nhân - Actors)

#### Tác nhân chính (Primary Actors)

* **Khách hàng (Customer):**
* Đăng ký, đăng nhập, cập nhật thông tin cá nhân.


* Nhập điểm đón/điểm đến, chọn loại xe và gửi yêu cầu đặt xe.


* Theo dõi trạng thái chuyến đi, vị trí tài xế và thời gian dự kiến tài xế đến.


* Xem lịch sử chuyến đi, số tiền cước, thực hiện thanh toán và đánh giá tài xế.




* **Tài xế (Driver):**
* Đăng ký/nhận tài khoản, cập nhật hồ sơ, thông tin phương tiện và trạng thái sẵn sàng nhận chuyến.


* Nhận thông báo chuyến đi phù hợp, chấp nhận hoặc từ chối chuyến.


* Cập nhật tiến trình chuyến đi (*Đã đến điểm đón*, *Đã đón khách*, *Đang di chuyển*, *Hoàn thành*).


* Chia sẻ dữ liệu vị trí theo thời gian thực về hệ thống.




* **Nhân viên vận hành (Operations / Admin Staff):**
* Quản lý tài khoản khách hàng, tài xế, phương tiện và thông tin chuyến đi.


* Theo dõi các chuyến đi đang diễn ra, trạng thái tài xế và hỗ trợ xử lý các chuyến đi bị lỗi.


* Tra cứu lịch sử giao dịch và xem báo cáo thống kê (doanh thu, tỷ lệ hoàn thành/hủy, hiệu quả hoạt động).


* Được phân quyền quản trị theo vai trò để đảm bảo an toàn cho các thao tác nhạy cảm.





#### Tác nhân bên ngoài (External Systems)

* **Nhà cung cấp thanh toán (Payment Provider):** Xử lý giao dịch thanh toán điện tử bên ngoài mà không lưu trực tiếp thông tin thẻ/tài khoản nhạy cảm vào hệ thống CAB.


* **Nhà cung cấp dịch vụ thông báo (Notification Provider):** Gửi thông báo Push Notification/SMS/Email đến khách hàng và tài xế.
