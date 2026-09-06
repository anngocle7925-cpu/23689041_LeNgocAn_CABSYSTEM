Bước 1: Tìm hiểu nghiệp vụ, hệ thống hiện tại có những vấn đề gì, mục tiêu, vấn đề hiện tại là gì, ai là người sử dụng hệ thống 

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



Bước 2: Xác định các stakeholder và vai trò


| STT | Bên liên quan (Stakeholder) | Phân loại | Vai trò và Trách nhiệm |
| :--- | :--- | :--- | :--- |
| 1 | **Ban lãnh đạo / Ban giám đốc** | Nội bộ | Khởi xướng và định hướng xây dựng nền tảng CAB mới để thay thế hệ thống cũ, với yêu cầu có khả năng mở rộng và phục vụ lượng lớn người dùng. Yêu cầu hệ thống cung cấp các báo cáo quản trị như doanh thu, số lượng chuyến, tỷ lệ hoàn thành/hủy chuyến và hiệu quả hoạt động của tài xế. |
| 2 | **Khách hàng** | Bên ngoài (Người dùng) | Đăng ký, đăng nhập và quản lý thông tin cá nhân trên ứng dụng. Gửi yêu cầu đặt xe, theo dõi vị trí tài xế và trạng thái chuyến đi. Trả tiền cước bằng tiền mặt hoặc thanh toán điện tử, xem lịch sử chuyến đi và đánh giá tài xế sau khi hoàn thành. |
| 3 | **Tài xế** | Bên ngoài (Người cung cấp dịch vụ) | Tạo tài khoản (hoặc được tạo hộ), cập nhật hồ sơ cá nhân và thông tin phương tiện. Bật trạng thái sẵn sàng, nhận thông báo chuyến mới, đưa ra quyết định chấp nhận hoặc từ chối chuyến đi. Cập nhật trạng thái trong suốt hành trình (đã đến điểm đón, đã đón khách, đang di chuyển, hoàn thành) và chia sẻ dữ liệu vị trí liên tục. |
| 4 | **Nhân viên vận hành** | Nội bộ | Sử dụng giao diện quản trị có phân quyền để quản lý dữ liệu khách hàng, tài xế, phương tiện và chuyến đi. Giám sát tiến trình các chuyến đang diễn ra, tra cứu lịch sử giao dịch và hỗ trợ xử lý khi có chuyến bị lỗi. |
| 5 | **Business Analyst (BA)** | Nội bộ (Đội dự án) | Phân tích và xác định rõ phạm vi, quy trình, yêu cầu chức năng/phi chức năng, quy tắc nghiệp vụ và các tác nhân của hệ thống. Chịu trách nhiệm làm việc với các bên liên quan để làm rõ các chi tiết chưa chốt (cách tính cước, tiêu chí tìm tài xế, chính sách hủy chuyến, v.v.) trước khi đội phát triển bắt tay vào làm. |
| 6 | **Nhóm phát triển** | Nội bộ (Đội dự án) | Chịu trách nhiệm xây dựng các giải pháp và phát triển hệ thống nền tảng CAB dựa trên những vấn đề đã được Business Analyst làm rõ. Đảm bảo kiến trúc linh hoạt để sau này có thể bổ sung dịch vụ, cổng thanh toán hoặc thông báo mà không cần xây dựng lại từ đầu. |
| 7 | **Nhà cung cấp thanh toán bên ngoài** | Đối tác thứ ba | Tích hợp vào hệ thống CAB để xử lý giao dịch điện tử và tính cước. Đảm bảo việc thanh toán diễn ra an toàn mà không yêu cầu hệ thống CAB phải lưu trữ trực tiếp các thông tin nhạy cảm của thẻ hoặc tài khoản người dùng. |
| 8 | **Nhà cung cấp thông báo** | Đối tác thứ ba | Cung cấp kênh gửi thông báo đến khách hàng và tài xế trong các cột mốc quan trọng (nhận chuyến, đến điểm đón, hoàn thành chuyến, kết quả thanh toán). Được tích hợp theo kiến trúc mở rộng để doanh nghiệp có thể thêm kênh thông báo mới trong tương lai. |


