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


### 3. Yêu cầu nghiệp vụ (Business Requirements)

#### 3.1. Mục tiêu kinh doanh (Business Objectives)
* **BO-01:** Tự động hóa hoàn toàn quy trình phân công tài xế, thay thế phương pháp thủ công hiện tại nhằm tối ưu hóa thời gian và chi phí vận hành[cite: 1].
* **BO-02:** Nâng cao trải nghiệm khách hàng thông qua việc cung cấp khả năng theo dõi trạng thái chuyến đi, dự kiến thời gian đến (ETA) và quản lý thanh toán tập trung[cite: 1].
* **BO-03:** Xây dựng nền tảng có kiến trúc linh hoạt, khả năng mở rộng cao để phục vụ số lượng lớn người dùng và dễ dàng tích hợp thêm dịch vụ mới trong tương lai[cite: 1].
* **BO-04:** Hoàn thành xây dựng và triển khai sản phẩm trong thời gian giới hạn là 7 tuần[cite: 1].

#### 3.2. Yêu cầu nghiệp vụ cốt lõi (Core Business Rules)
* **BR-01 (Điều phối & Tìm tài xế):** Khi có yêu cầu đặt xe, hệ thống phải tự động tìm và ưu tiên tài xế phù hợp, gần khách hàng nhất dựa trên vị trí và trạng thái sẵn sàng[cite: 1].
* **BR-02 (Cơ chế chuyển tiếp chuyến):** Nếu tài xế đầu tiên từ chối hoặc không phản hồi, hệ thống phải tự động tìm tài xế khác thay thế mà không yêu cầu khách hàng tạo lại chuyến[cite: 1]. Nếu hoàn toàn không tìm được tài xế, phải thông báo rõ ràng cho khách hàng[cite: 1].
* **BR-03 (Quản lý trạng thái chuyến đi):** Tài xế bắt buộc phải cập nhật tuần tự các trạng thái của chuyến đi (đã đến điểm đón, đã đón khách, đang di chuyển, hoàn thành chuyến) và liên tục gửi vị trí về hệ thống[cite: 1].
* **BR-04 (Thanh toán & Cước phí):** Hệ thống tự động tính cước dựa trên loại dịch vụ và thông tin chuyến đi sau khi hoàn thành[cite: 1]. Quá trình thanh toán điện tử phải được thực hiện qua bên thứ 3, tuyệt đối không lưu thông tin thẻ/tài khoản ngân hàng của khách hàng vào hệ thống CAB[cite: 1].
* **BR-05 (Xử lý giao dịch lỗi):** Trong trường hợp thanh toán điện tử thất bại, hệ thống phải thông báo ngay cho khách hàng và cung cấp cơ chế xử lý lại giao dịch[cite: 1].
* **BR-06 (Thông báo):** Hệ thống phải tự động kích hoạt thông báo (Push/SMS) tại các cột mốc: tiếp nhận yêu cầu, có tài xế nhận chuyến, tài xế đến điểm đón, hoàn thành chuyến và có kết quả thanh toán[cite: 1].
* **BR-07 (Bảo mật & Lưu vết):** Mọi thao tác quản trị phải được kiểm soát quyền truy cập (RBAC)[cite: 1]. Hệ thống phải lưu vết (log) các thao tác quan trọng để phục vụ đối soát, kiểm tra khi có sự cố[cite: 1].

#### 3.3. Các quy tắc nghiệp vụ cần làm rõ thêm (Pending Business Rules)
*(Lưu ý: Các mục dưới đây là những điểm BA cần chốt lại với doanh nghiệp trước khi phát triển)*
* **PR-01:** Công thức và chi tiết cách tính cước phí chuyến đi[cite: 1].
* **PR-02:** Bộ tiêu chí cụ thể để ưu tiên tài xế và thời gian tối đa (timeout) để tài xế phản hồi yêu cầu nhận chuyến[cite: 1].
* **PR-03:** Chính sách hủy chuyến (phí phạt, điều kiện hủy) của khách hàng và tài xế[cite: 1].
* **PR-04:** Kịch bản xử lý nghiệp vụ khi thiết bị của tài xế hoặc khách hàng mất kết nối mạng giữa chuyến đi[cite: 1].
* **PR-05:** Quy định về thời gian lưu trữ dữ liệu (Data Retention) của hệ thống[cite: 1].

### 4. Quy trình nghiệp vụ cốt lõi (Core Business Processes)

#### 4.1. Quy trình Đặt xe & Điều phối (Booking & Matching Process)
1. **Khách hàng** đăng nhập vào ứng dụng, nhập điểm đón, điểm đến và lựa chọn loại xe mong muốn[cite: 1].
2. Hệ thống tiếp nhận yêu cầu đặt xe và khởi tạo trạng thái tìm kiếm[cite: 1].
3. Dịch vụ điều phối (**Matching Service**) tự động tìm kiếm các tài xế phù hợp đang ở trạng thái sẵn sàng và ở gần khu vực đón khách nhất dựa trên tọa độ vị trí[cite: 1].
4. Hệ thống gửi thông báo mời nhận chuyến đến tài xế ưu tiên đầu tiên[cite: 1].
    * *Trường hợp 1:* Tài xế bấm **Chấp nhận** $\rightarrow$ Hệ thống gán chuyến cho tài xế đó và cập nhật thông tin cho khách hàng.
    * *Trường hợp 2:* Tài xế **Từ chối** hoặc **Không phản hồi** trong thời gian quy định $\rightarrow$ Hệ thống tự động chuyển sang đề xuất tài xế phù hợp tiếp theo mà không làm gián đoạn yêu cầu của khách[cite: 1].
    * *Trường hợp 3:* Đã quét hết danh sách nhưng hoàn toàn không tìm thấy tài xế $\rightarrow$ Hệ thống thông báo rõ ràng cho khách hàng[cite: 1].

#### 4.2. Quy trình Thực hiện chuyến đi (Trip Execution Process)
1. Sau khi nhận chuyến, tài xế di chuyển đến điểm đón của khách và cập nhật trạng thái **"Đã đến điểm đón"** (hệ thống gửi thông báo cho khách)[cite: 1].
2. Khi khách lên xe, tài xế cập nhật trạng thái **"Đã đón khách / Đang di chuyển"**[cite: 1].
3. Trong suốt quá trình di chuyển, ứng dụng của tài xế liên tục gửi dữ liệu vị trí (GPS) về hệ thống để khách hàng theo dõi hành trình và cập nhật thời gian dự kiến đến (ETA)[cite: 1].
4. Khi đến nơi, tài xế cập nhật trạng thái **"Hoàn thành chuyến"**[cite: 1].

#### 4.3. Quy trình Tính cước & Thanh toán (Billing & Payment Process)
1. Ngay khi chuyến đi hoàn thành, hệ thống tính toán số tiền cước dựa trên loại dịch vụ và thông tin hành trình[cite: 1].
2. Khách hàng lựa chọn hình thức thanh toán:
    * **Tiền mặt:** Thanh toán trực tiếp cho tài xế.
    * **Thanh toán điện tử:** Hệ thống chuyển hướng yêu cầu qua **Cổng thanh toán bên ngoài (Third-party Payment Provider)**. Thông tin nhạy cảm của thẻ/tài khoản được xử lý bảo mật bên ngoài, không lưu trực tiếp trên hệ thống CAB[cite: 1].
3. *Xử lý ngoại lệ:* Nếu giao dịch điện tử thất bại, hệ thống gửi thông báo lỗi ngay cho khách hàng và cung cấp tuỳ chọn thanh toán lại theo chính sách của doanh nghiệp[cite: 1].

#### 4.4. Quy trình Đánh giá & Hậu kỳ (Rating & Operations Process)
1. Sau khi hoàn tất thanh toán, khách hàng thực hiện đánh giá (rating) chất lượng tài xế[cite: 1].
2. Bộ phận **Vận hành (Operations)** sử dụng giao diện quản trị để theo dõi các chuyến đi đang diễn ra, kiểm tra trạng thái tài xế, xử lý các sự cố phát sinh (như khiếu nại, lỗi chuyến) và tra cứu lịch sử giao dịch[cite: 1].
3. Hệ thống tổng hợp dữ liệu để trích xuất các báo cáo quản trị phục vụ Ban lãnh đạo (gồm tổng số chuyến, doanh thu, tỷ lệ hoàn thành chuyến, tỷ lệ hủy và hiệu quả hoạt động của tài xế)[cite: 1].

### 5. Mô hình hóa nghiệp vụ (Business Modeling)

#### 5.1. Biểu đồ Use Case tổng quan (System Use Case Diagram)
Biểu đồ Use Case mô tả mối quan hệ giữa các tác nhân (Actors) và các chức năng (Use Cases) mà hệ thống CAB cung cấp:

* **Tác nhân Khách hàng (Customer):**
  * `UC01`: Đăng ký / Đăng nhập / Quản lý tài khoản
  * `UC02`: Nhập điểm đón / điểm đi & Chọn loại xe
  * `UC03`: Gửi yêu cầu đặt xe
  * `UC04`: Theo dõi trạng thái chuyến đi & Vị trí tài xế (Real-time tracking)
  * `UC05`: Thanh toán cước phí (Tiền mặt / Điện tử)
  * `UC06`: Đánh giá tài xế sau chuyến đi

* **Tác nhân Tài xế (Driver):**
  * `UC07`: Quản lý hồ sơ & Thông tin phương tiện
  * `UC08`: Cập nhật trạng thái sẵn sàng (Online/Offline)
  * `UC09`: Nhận thông báo & Chấp nhận/Từ chối chuyến đi
  * `UC10`: Cập nhật tiến trình chuyến đi (Đến điểm đón, Đón khách, Hoàn thành)
  * `UC11`: Gửi tọa độ vị trí định kỳ về hệ thống

* **Tác nhân Nhân viên vận hành (Operations Staff):**
  * `UC12`: Quản lý người dùng (Khách hàng, Tài xế, Phương tiện)
  * `UC13`: Giám sát chuyến đi đang diễn ra & Xử lý lỗi chuyến
  * `UC14`: Tra cứu lịch sử giao dịch
  * `UC15`: Xem báo cáo thống kê (Doanh thu, Tỷ lệ hoàn thành/hủy, Hiệu suất tài xế)

* **Tác nhân Hệ thống thanh toán bên ngoài (Third-party Payment Gateway):**
  * `UC16`: Xử lý giao dịch thanh toán điện tử (Bảo mật thông tin thẻ)

---

#### 5.2. Biểu đồ hoạt động: Luồng Đặt xe & Điều phối tài xế (Activity Diagram - Booking & Matching)
Mô tả chi tiết các bước xử lý khi một khách hàng tiến hành đặt xe trên nền tảng CAB:

1. **Bắt đầu:** Khách hàng nhập điểm đón, điểm đến, chọn loại xe và xác nhận tạo yêu cầu đặt xe.
2. **Khởi tạo:** Hệ thống tiếp nhận yêu cầu, sinh mã chuyến đi và chuyển sang trạng thái tìm kiếm tài xế (`Searching`).
3. **Thuật toán Matching:** 
   * Hệ thống quét danh sách tài xế đang ở trạng thái `Ready` (Sẵn sàng) và nằm trong bán kính/khu vực phù hợp.
   * Sắp xếp danh sách ưu tiên theo khoảng cách gần khách hàng nhất.
4. **Gửi đề xuất:** Hệ thống gửi thông báo mời nhận chuyến đến tài xế đứng đầu danh sách ưu tiên.
5. **Kiểm tra phản hồi của tài xế:**
   * *Trường hợp A (Chấp nhận):* Tài xế bấm nhận chuyến trong thời gian quy định $\rightarrow$ Hệ thống gán chuyến cho tài xế, cập nhật trạng thái chuyến đi thành `Driver Assigned` và thông báo cho khách hàng. Chuyển sang giai đoạn thực hiện chuyến.
   * *Trường hợp B (Từ chối / Hết giờ - Timeout):* Tài xế từ chối hoặc không phản hồi $\rightarrow$ Hệ thống loại bỏ tài xế này khỏi danh sách hiện tại và chuyển sang đề xuất tài xế ưu tiên tiếp theo (Lặp lại bước 4).
   * *Trường hợp C (Hết toàn bộ danh sách mà không ai nhận):* Hệ thống thông báo lỗi không tìm được tài xế cho khách hàng (`No Driver Found`) và kết thúc quy trình.
  
```mermaid
flowchart TD
    A[Khách hàng nhập điểm đón, điểm đi và chọn loại xe] --> B[Hệ thống tạo yêu cầu và khởi tạo trạng thái tìm kiếm]
    B --> C{Tim tai xe}
    C -->|Khong tim thay| D[Thong bao loi: Khong tim thay tai xe] --> E([Ket thuc])
    C -->|Tim thay danh sach| F[Gui thong bao moi nhan chuyen cho tai xe uu tien so 1]
    
    F --> G{Tai xe phan hoi?}
    G -->|Tu choi hoac Het gio Timeout| H[Loai tai xe nay khoi danh sach hien tai]
    H --> I{Con tai xe khac trong danh sach?}
    I -->|Con| F
    I -->|Het| D
    
    G -->|Chap nhan| J[Hệ thống gán chuyến cho tài xế và Bắt đầu hành trình]

    J --> K[Tài xế di chuyển đến điểm đón và Cập nhật: Đã đến điểm đón]
    K --> L[Đón khách và Cập nhật: Đang di chuyển]
    L --> M[Đến nơi và Cập nhật: Hoàn thành chuyến đi]
    M --> N[Hệ thống tính cước dựa trên loại dịch vụ và hành trình]
    
    N --> O{Hinh thuc thanh toan?}
    O -->|Tien mat| P[Khách trả trực tiếp cho tài xế] --> Q([Hoàn tất giao dịch và Đánh giá])
    O -->|Dien tu| R[Chuyển hướng qua Cổng thanh toán bên ngoài]
    
    R --> S{Giao dich thanh cong?}
    S -->|Thanh cong| T[Xác nhận thanh toán thành công] --> Q
    S -->|That bai| U[Thông báo lỗi thanh toán và Cho phép xử lý lại] --> Q




