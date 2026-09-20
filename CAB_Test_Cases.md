# CAB System – Test Cases
> Bộ test case được xây dựng từ các Use Case, BR, FR và QT đã có trong `srs.md`. Phạm vi chỉ bao gồm nghiệp vụ CAB đã được mô tả trong SRS; không tự phát sinh nghiệp vụ ngoài phạm vi.
## 1. Phạm vi kiểm thử
Bộ test bao phủ **19 Use Case (UC-01 → UC-19)** trong SRS. Các yêu cầu NFR thuần kiến trúc như chịu tải, fault isolation, deploy độc lập và bảo mật chuyên sâu không được biến thành test case chức năng nếu SRS quy định phải kiểm chứng bằng load/stress/security/fault-injection/CI-CD.
## 2. Quy ước
- **Positive:** dữ liệu/điều kiện hợp lệ, kiểm tra luồng thành công.
- **Negative:** dữ liệu hoặc thao tác không hợp lệ.
- **Validation/Empty:** kiểm tra trường bắt buộc, dữ liệu không hợp lệ.
- **Business Rule:** kiểm tra các QT được nêu trong SRS.
- **Exception/Error:** chỉ đưa vào khi có luồng ngoại lệ/lỗi được SRS mô tả.
- Các tham số SRS còn **TBD/chưa chốt** không được tự gán giá trị cụ thể.
## UC-01 – Đăng ký / Đăng nhập
**Actor:** Khách hàng, Tài xế

| Test Case ID | Test Scenario | Preconditions | Test Steps | Test Data | Expected Result |
|---|---|---|---|---|---|
| TC-01-01 | Đăng nhập với thông tin hợp lệ | Tài khoản đã đăng ký; thông tin xác thực hợp lệ | Nhập thông tin hợp lệ → chọn Đăng nhập | Dữ liệu hợp lệ | Đăng nhập thành công và tạo session hợp lệ. |
| TC-01-02 | Đăng nhập sai mật khẩu | Tài khoản tồn tại | Nhập đúng tài khoản, sai mật khẩu → Đăng nhập | Dữ liệu theo ngữ cảnh kiểm thử | Hệ thống từ chối đăng nhập. |
| TC-01-03 | Đăng nhập với tài khoản chưa tồn tại | Đang ở màn hình đăng nhập | Nhập tài khoản chưa đăng ký → Đăng nhập | Dữ liệu theo ngữ cảnh kiểm thử | Hệ thống từ chối đăng nhập. |
| TC-01-04 | Bỏ trống thông tin đăng nhập | Đang ở màn hình đăng nhập | Để trống thông tin bắt buộc → Đăng nhập | Dữ liệu theo ngữ cảnh kiểm thử | Hệ thống không tạo session và yêu cầu bổ sung thông tin. |
| TC-01-05 | Đăng nhập với thông tin không hợp lệ | Đang ở màn hình đăng nhập | Nhập dữ liệu xác thực không hợp lệ → Đăng nhập | Dữ liệu hợp lệ | Hệ thống từ chối xác thực. |
| TC-01-06 | Đăng ký tài khoản hợp lệ | Chưa có tài khoản | Nhập đầy đủ thông tin đăng ký hợp lệ → Gửi đăng ký | Dữ liệu hợp lệ | Tài khoản được tạo; người dùng có thể thực hiện xác thực. |
| TC-01-07 | Đăng ký với dữ liệu bắt buộc bị bỏ trống | Đang ở chức năng đăng ký | Bỏ trống trường bắt buộc → Gửi đăng ký | Dữ liệu theo ngữ cảnh kiểm thử | Hệ thống không tạo tài khoản và yêu cầu nhập đủ dữ liệu. |

## UC-02 – Cập nhật hồ sơ khách hàng
**Actor:** Khách hàng

| Test Case ID | Test Scenario | Preconditions | Test Steps | Test Data | Expected Result |
|---|---|---|---|---|---|
| TC-02-01 | Cập nhật hồ sơ với dữ liệu hợp lệ | Khách hàng đã đăng nhập | Sửa thông tin hợp lệ → Lưu | Dữ liệu hợp lệ | Thông tin hồ sơ được cập nhật. |
| TC-02-02 | Lưu hồ sơ khi trường bắt buộc bị bỏ trống | Đã đăng nhập | Xóa dữ liệu bắt buộc → Lưu | Dữ liệu theo ngữ cảnh kiểm thử | Hệ thống từ chối cập nhật nếu trường đó bắt buộc. |
| TC-02-03 | Nhập dữ liệu hồ sơ không hợp lệ | Đã đăng nhập | Nhập dữ liệu sai định dạng → Lưu | Dữ liệu hợp lệ | Hệ thống báo dữ liệu không hợp lệ và không lưu. |
| TC-02-04 | Cập nhật hồ sơ không thay đổi dữ liệu | Đã đăng nhập | Mở hồ sơ → Lưu mà không sửa | Dữ liệu theo ngữ cảnh kiểm thử | Hệ thống xử lý hợp lệ, không tạo dữ liệu sai lệch. |
| TC-02-05 | Cập nhật khi chưa đăng nhập | Chưa có session hợp lệ | Truy cập chức năng cập nhật hồ sơ | Dữ liệu theo ngữ cảnh kiểm thử | Hệ thống yêu cầu xác thực và không cho cập nhật. |

## UC-03 – Đặt xe
**Actor:** Khách hàng

| Test Case ID | Test Scenario | Preconditions | Test Steps | Test Data | Expected Result |
|---|---|---|---|---|---|
| TC-03-01 | Đặt xe với dữ liệu hợp lệ | Đã đăng nhập; không có chuyến đang hoạt động | Nhập điểm đón, điểm đến, loại xe → Xác nhận | Dữ liệu hợp lệ | Trip được tạo ở trạng thái 'Đang tìm tài xế' và kích hoạt Matching. |
| TC-03-02 | Bỏ trống điểm đón | Đã đăng nhập | Để trống điểm đón → Xác nhận | Dữ liệu theo ngữ cảnh kiểm thử | Không tạo Trip; yêu cầu nhập điểm đón. |
| TC-03-03 | Bỏ trống điểm đến | Đã đăng nhập | Để trống điểm đến → Xác nhận | Dữ liệu theo ngữ cảnh kiểm thử | Không tạo Trip; yêu cầu nhập điểm đến. |
| TC-03-04 | Không chọn loại xe | Đã đăng nhập | Nhập điểm đón/đến nhưng không chọn loại xe → Xác nhận | Dữ liệu theo ngữ cảnh kiểm thử | Hệ thống từ chối nếu loại xe là dữ liệu bắt buộc của yêu cầu đặt xe. |
| TC-03-05 | Điểm đón/đến không hợp lệ | Đã đăng nhập | Nhập địa điểm ngoài vùng phục vụ → Xác nhận | Dữ liệu hợp lệ | Hệ thống báo lỗi và yêu cầu nhập lại. |
| TC-03-06 | Khách hàng đang có chuyến chưa hoàn thành | Có active trip | Thực hiện đặt chuyến mới | Dữ liệu theo ngữ cảnh kiểm thử | Hệ thống từ chối tạo chuyến mới. |
| TC-03-07 | Tạo chuyến thành công và gọi Matching | Đã đăng nhập; dữ liệu hợp lệ | Xác nhận đặt xe → kiểm tra trạng thái | Dữ liệu hợp lệ | Trip ở 'Đang tìm tài xế'; Matching được kích hoạt. |

## UC-04 – Theo dõi chuyến đi
**Actor:** Khách hàng

| Test Case ID | Test Scenario | Preconditions | Test Steps | Test Data | Expected Result |
|---|---|---|---|---|---|
| TC-04-01 | Theo dõi chuyến đang hoạt động | Customer có trip đang hoạt động | Mở màn hình theo dõi | Dữ liệu theo ngữ cảnh kiểm thử | Hiển thị trạng thái chuyến theo dữ liệu Trip. |
| TC-04-02 | Hiển thị vị trí tài xế | Có driver đang thực hiện chuyến và có dữ liệu location | Mở theo dõi chuyến | Dữ liệu theo ngữ cảnh kiểm thử | Vị trí tài xế được cập nhật từ dữ liệu Driver Location. |
| TC-04-03 | Theo dõi trạng thái theo thời gian thực | Trip đang thay đổi trạng thái | Mở màn hình theo dõi và quan sát cập nhật | Dữ liệu theo ngữ cảnh kiểm thử | Trạng thái được cập nhật gần thời gian thực theo yêu cầu SRS. |
| TC-04-04 | Theo dõi chuyến không thuộc khách hàng | Đã đăng nhập | Truy cập dữ liệu trip của khách khác | Dữ liệu theo ngữ cảnh kiểm thử | Hệ thống không cho xem dữ liệu không thuộc quyền truy cập. |
| TC-04-05 | Theo dõi chuyến đã hoàn thành | Trip đã hoàn thành | Mở thông tin chuyến | Dữ liệu theo ngữ cảnh kiểm thử | Hiển thị trạng thái cuối cùng và thông tin chuyến phù hợp. |

## UC-05 – Thanh toán
**Actor:** Khách hàng; Cổng thanh toán bên thứ ba

| Test Case ID | Test Scenario | Preconditions | Test Steps | Test Data | Expected Result |
|---|---|---|---|---|---|
| TC-05-01 | Thanh toán tiền mặt thành công | Trip đã hoàn thành; cước đã tính | Chọn tiền mặt → Driver xác nhận nhận tiền | Dữ liệu hợp lệ | Payment.status = success; Trip được đánh dấu đã thanh toán. |
| TC-05-02 | Thanh toán điện tử thành công | Trip đã hoàn thành; cước đã tính | Chọn điện tử → gọi gateway → gateway trả success | Dữ liệu hợp lệ | Payment.status = success; Trip được đánh dấu đã thanh toán. |
| TC-05-03 | Thanh toán điện tử thất bại | Trip đã hoàn thành | Chọn điện tử → gateway trả failed | Dữ liệu theo ngữ cảnh kiểm thử | Payment.status = failed; cho phép retry hoặc chuyển sang tiền mặt theo QT-12. |
| TC-05-04 | Gateway không phản hồi | Trip đã hoàn thành | Gửi yêu cầu điện tử → gateway timeout | Dữ liệu theo ngữ cảnh kiểm thử | Xử lý như thất bại và ghi log để đối soát. |
| TC-05-05 | Thanh toán khi chuyến chưa hoàn thành | Trip chưa hoàn thành | Thử thanh toán | Dữ liệu theo ngữ cảnh kiểm thử | Hệ thống không cho thanh toán vì không thỏa tiền điều kiện. |
| TC-05-06 | Không lưu thông tin thanh toán nhạy cảm | Đang xử lý thanh toán điện tử | Thực hiện giao dịch → kiểm tra dữ liệu hệ thống | Dữ liệu theo ngữ cảnh kiểm thử | Hệ thống không lưu thông tin thẻ/tài khoản nhạy cảm. |
| TC-05-07 | Tính cước sau khi hoàn thành | Trip vừa hoàn thành | Kiểm tra xử lý Payment | Dữ liệu theo ngữ cảnh kiểm thử | Hệ thống tính cước dựa trên distance_km và duration_min; công thức cụ thể theo QT-10 chưa chốt. |

## UC-06 – Xem lịch sử chuyến
**Actor:** Khách hàng

| Test Case ID | Test Scenario | Preconditions | Test Steps | Test Data | Expected Result |
|---|---|---|---|---|---|
| TC-06-01 | Xem lịch sử có dữ liệu | Đã đăng nhập; có lịch sử | Mở lịch sử chuyến | Dữ liệu theo ngữ cảnh kiểm thử | Danh sách chuyến của khách được hiển thị. |
| TC-06-02 | Xem lịch sử khi không có dữ liệu | Đã đăng nhập; chưa có trip | Mở lịch sử | Dữ liệu theo ngữ cảnh kiểm thử | Hệ thống hiển thị trạng thái không có dữ liệu. |
| TC-06-03 | Lọc lịch sử theo thời gian hợp lệ | Có nhiều trip | Nhập khoảng thời gian hợp lệ → lọc | Dữ liệu hợp lệ | Chỉ hiển thị các chuyến phù hợp khoảng thời gian. |
| TC-06-04 | Lọc với khoảng thời gian không hợp lệ | Đã đăng nhập | Nhập khoảng thời gian không hợp lệ → lọc | Dữ liệu hợp lệ | Hệ thống từ chối hoặc yêu cầu nhập lại khoảng thời gian. |
| TC-06-05 | Xem lịch sử khi chưa đăng nhập | Chưa có session | Truy cập lịch sử | Dữ liệu theo ngữ cảnh kiểm thử | Hệ thống yêu cầu đăng nhập. |

## UC-07 – Đánh giá tài xế
**Actor:** Khách hàng

| Test Case ID | Test Scenario | Preconditions | Test Steps | Test Data | Expected Result |
|---|---|---|---|---|---|
| TC-07-01 | Đánh giá sau chuyến hợp lệ | Trip đã hoàn thành; chưa đánh giá | Chọn số sao 1–5 → nhập nhận xét tùy chọn → gửi | Dữ liệu hợp lệ | Tạo RATING và cập nhật rating_avg của tài xế. |
| TC-07-02 | Đánh giá mức sao thấp nhất hợp lệ | Trip đã hoàn thành; chưa đánh giá | Chọn 1 sao → gửi | Dữ liệu hợp lệ | Hệ thống ghi nhận đánh giá. |
| TC-07-03 | Đánh giá mức sao cao nhất hợp lệ | Trip đã hoàn thành; chưa đánh giá | Chọn 5 sao → gửi | Dữ liệu hợp lệ | Hệ thống ghi nhận đánh giá. |
| TC-07-04 | Đánh giá trước khi hoàn thành | Trip chưa hoàn thành | Thử đánh giá | Dữ liệu theo ngữ cảnh kiểm thử | Hệ thống từ chối theo QT-17. |
| TC-07-05 | Đánh giá lại cùng chuyến | Trip đã được đánh giá | Gửi rating lần hai | Dữ liệu theo ngữ cảnh kiểm thử | Hệ thống từ chối theo QT-18. |
| TC-07-06 | Bỏ qua nhận xét tùy chọn | Trip đã hoàn thành; chưa đánh giá | Chọn sao, để trống nhận xét → gửi | Dữ liệu theo ngữ cảnh kiểm thử | Rating được ghi nhận vì nhận xét là tùy chọn. |

## UC-08 – Hủy chuyến
**Actor:** Khách hàng; Tài xế nếu đã được gán

| Test Case ID | Test Scenario | Preconditions | Test Steps | Test Data | Expected Result |
|---|---|---|---|---|---|
| TC-08-01 | Hủy khi đang tìm tài xế | Trip = Đang tìm tài xế | Chọn Hủy → xác nhận | Dữ liệu theo ngữ cảnh kiểm thử | Trip chuyển 'Đã hủy' và ghi cancelled_reason. |
| TC-08-02 | Hủy khi đã có tài xế | Trip = Đã có tài xế | Chọn Hủy → xác nhận | Dữ liệu theo ngữ cảnh kiểm thử | Trip chuyển 'Đã hủy'; tài xế được thông báo và quay lại trạng thái sẵn sàng theo luồng SRS. |
| TC-08-03 | Hủy khi đang di chuyển | Trip = Đang di chuyển | Chọn Hủy | Dữ liệu theo ngữ cảnh kiểm thử | Hệ thống từ chối hủy qua UC-08 và hiển thị thông báo. |
| TC-08-04 | Hủy không nhập lý do | Trip ở trạng thái được phép hủy | Chọn Hủy → bỏ trống lý do → xác nhận | Dữ liệu theo ngữ cảnh kiểm thử | Hủy được xử lý vì lý do được mô tả là tùy chọn. |
| TC-08-05 | Ghi nhận lý do hủy | Trip được phép hủy | Nhập lý do → xác nhận | Dữ liệu theo ngữ cảnh kiểm thử | cancelled_reason được ghi nhận và dữ liệu hủy được dùng cho thống kê theo QT-16. |

## UC-09 – Cập nhật hồ sơ & xe (tài xế)
**Actor:** Tài xế

| Test Case ID | Test Scenario | Preconditions | Test Steps | Test Data | Expected Result |
|---|---|---|---|---|---|
| TC-09-01 | Cập nhật hồ sơ và xe hợp lệ | Driver đã đăng nhập | Nhập đầy đủ thông tin hợp lệ → Lưu | Dữ liệu hợp lệ | Hồ sơ và thông tin xe được cập nhật. |
| TC-09-02 | Bỏ trống thông tin xe bắt buộc | Driver đã đăng nhập | Xóa trường xe bắt buộc → Lưu | Dữ liệu theo ngữ cảnh kiểm thử | Hệ thống không lưu dữ liệu không hợp lệ. |
| TC-09-03 | Thông tin giấy tờ không hợp lệ | Driver đã đăng nhập | Nhập dữ liệu giấy tờ không hợp lệ → Lưu | Dữ liệu hợp lệ | Hệ thống validate và từ chối dữ liệu. |
| TC-09-04 | Cập nhật hồ sơ khi chưa đăng nhập | Chưa có session | Truy cập chức năng | Dữ liệu theo ngữ cảnh kiểm thử | Hệ thống yêu cầu xác thực. |
| TC-09-05 | Cập nhật thành công và đủ điều kiện ready | Driver có dữ liệu xe đầy đủ | Lưu hồ sơ/xe hợp lệ | Dữ liệu hợp lệ | Driver đạt điều kiện dữ liệu để bật trạng thái sẵn sàng theo QT-07. |

## UC-10 – Bật/tắt trạng thái sẵn sàng
**Actor:** Tài xế

| Test Case ID | Test Scenario | Preconditions | Test Steps | Test Data | Expected Result |
|---|---|---|---|---|---|
| TC-10-01 | Bật Ready khi hồ sơ/xe đầy đủ | Driver đã có đủ thông tin theo QT-07 | Bật trạng thái sẵn sàng | Dữ liệu theo ngữ cảnh kiểm thử | Driver chuyển sang Ready. |
| TC-10-02 | Tắt Ready | Driver đang Ready | Tắt trạng thái sẵn sàng | Dữ liệu theo ngữ cảnh kiểm thử | Driver chuyển khỏi trạng thái Ready. |
| TC-10-03 | Bật Ready khi thiếu thông tin xe | Driver thiếu thông tin cần thiết | Bật Ready | Dữ liệu theo ngữ cảnh kiểm thử | Hệ thống từ chối theo QT-07. |
| TC-10-04 | Bật Ready khi chưa xác thực | Chưa đăng nhập | Thử bật Ready | Dữ liệu theo ngữ cảnh kiểm thử | Hệ thống từ chối và yêu cầu xác thực. |

## UC-11 – Nhận / Từ chối chuyến
**Actor:** Tài xế

| Test Case ID | Test Scenario | Preconditions | Test Steps | Test Data | Expected Result |
|---|---|---|---|---|---|
| TC-11-01 | Chấp nhận chuyến | Driver Ready; có pending matching attempt | Mở yêu cầu → Chấp nhận | Dữ liệu theo ngữ cảnh kiểm thử | Attempt = accepted; Trip được gán driver và chuyển 'Đã có tài xế'. |
| TC-11-02 | Từ chối chuyến | Driver Ready; có pending attempt | Mở yêu cầu → Từ chối | Dữ liệu theo ngữ cảnh kiểm thử | Attempt = rejected; driver vẫn sẵn sàng; Matching có thể fallback. |
| TC-11-03 | Không phản hồi | Có pending attempt | Không thao tác đến khi hết thời gian | Dữ liệu theo ngữ cảnh kiểm thử | Attempt = timeout và Matching chuyển sang driver tiếp theo. |
| TC-11-04 | Chấp nhận khi không có pending attempt | Không có yêu cầu hợp lệ | Thử chấp nhận | Dữ liệu theo ngữ cảnh kiểm thử | Hệ thống không cho nhận yêu cầu không tồn tại/hết hiệu lực. |

## UC-12 – Cập nhật trạng thái chuyến
**Actor:** Tài xế

| Test Case ID | Test Scenario | Preconditions | Test Steps | Test Data | Expected Result |
|---|---|---|---|---|---|
| TC-12-01 | Đã có tài xế → Đến điểm đón | Driver đã nhận trip | Chọn 'Đã đến điểm đón' | Dữ liệu theo ngữ cảnh kiểm thử | Trip chuyển trạng thái và Customer nhận thông báo. |
| TC-12-02 | Đã đến điểm đón → Đã đón khách | Trip đang ở Đã đến điểm đón | Chọn 'Đã đón khách' | Dữ liệu theo ngữ cảnh kiểm thử | Trip chuyển sang trạng thái tương ứng và ghi started_at khi chuyển sang di chuyển theo luồng SRS. |
| TC-12-03 | Đã đón khách → Đang di chuyển | Trip đã đón khách | Cập nhật trạng thái tiếp theo | Dữ liệu theo ngữ cảnh kiểm thử | Trip chuyển 'Đang di chuyển'. |
| TC-12-04 | Đang di chuyển → Hoàn thành | Trip đang di chuyển | Chọn Hoàn thành | Dữ liệu theo ngữ cảnh kiểm thử | Trip = Hoàn thành; ghi completed_at, distance_km và kích hoạt UC-05. |
| TC-12-05 | Bỏ qua trạng thái | Trip chưa đủ điều kiện | Thử chuyển thẳng sang trạng thái sau | Dữ liệu theo ngữ cảnh kiểm thử | Hệ thống từ chối theo QT-09. |
| TC-12-06 | Cập nhật khi chưa nhận chuyến | Trip chưa được driver chấp nhận | Thử cập nhật trạng thái | Dữ liệu theo ngữ cảnh kiểm thử | Hệ thống từ chối vì không thỏa tiền điều kiện UC-12. |

## UC-13 – Gửi vị trí
**Actor:** Tài xế

| Test Case ID | Test Scenario | Preconditions | Test Steps | Test Data | Expected Result |
|---|---|---|---|---|---|
| TC-13-01 | Gửi vị trí hợp lệ định kỳ | Driver đang trong chuyến | Gửi dữ liệu GPS hợp lệ theo chu kỳ | Dữ liệu hợp lệ | Hệ thống ghi nhận vị trí vào DRIVER_LOCATION_LOG theo QT-08. |
| TC-13-02 | Gửi dữ liệu vị trí không hợp lệ | Driver đang hoạt động | Gửi location payload không hợp lệ | Dữ liệu hợp lệ | Hệ thống không ghi nhận dữ liệu vị trí không hợp lệ. |
| TC-13-03 | Không gửi vị trí trong chu kỳ | Driver đang trong chuyến | Không gửi dữ liệu theo chu kỳ | Dữ liệu theo ngữ cảnh kiểm thử | Hệ thống không tạo bản ghi vị trí giả; xử lý thiếu dữ liệu theo thiết kế. |
| TC-13-04 | Gửi vị trí khi chưa có chuyến | Driver không có active trip | Gửi location | Dữ liệu theo ngữ cảnh kiểm thử | Hệ thống xử lý theo điều kiện của UC; không gán vị trí vào một trip không tồn tại. |

## UC-14 – Tìm và phân công tài xế (Matching)
**Actor:** Matching Service; Tài xế

| Test Case ID | Test Scenario | Preconditions | Test Steps | Test Data | Expected Result |
|---|---|---|---|---|---|
| TC-14-01 | Chọn tài xế Ready phù hợp | Trip = Đang tìm tài xế | Kích hoạt Matching | Dữ liệu theo ngữ cảnh kiểm thử | Hệ thống lấy tài xế đang Ready theo tiêu chí matching. |
| TC-14-02 | Ưu tiên tài xế gần điểm đón | Có nhiều driver Ready | Chạy Matching | Dữ liệu theo ngữ cảnh kiểm thử | Hệ thống áp dụng tiêu chí gần điểm đón theo QT-02 hiện được SRS xem là giả định cần xác nhận. |
| TC-14-03 | Tạo matching attempt và gửi yêu cầu | Có driver phù hợp | Kích hoạt Matching | Dữ liệu theo ngữ cảnh kiểm thử | MATCHING_ATTEMPT được tạo và yêu cầu gửi cho driver. |
| TC-14-04 | Driver chấp nhận | Có pending attempt | Driver Accept | Dữ liệu theo ngữ cảnh kiểm thử | Trip.driver_id được cập nhật; Trip = Đã có tài xế; Customer được thông báo. |
| TC-14-05 | Driver từ chối | Có pending attempt | Driver Reject | Dữ liệu theo ngữ cảnh kiểm thử | Attempt = rejected; hệ thống fallback sang driver tiếp theo. |
| TC-14-06 | Driver timeout | Có pending attempt | Không phản hồi trong thời gian giới hạn QT-03 | Dữ liệu theo ngữ cảnh kiểm thử | Attempt = timeout; hệ thống fallback. |
| TC-14-07 | Không còn driver sau giới hạn thử | Các attempt đều thất bại | Tiếp tục Matching đến giới hạn QT-05 | Dữ liệu theo ngữ cảnh kiểm thử | Trip = Không tìm được tài xế; Customer được thông báo. |
| TC-14-08 | Không có driver Ready | Trip đang tìm tài xế | Chạy Matching | Dữ liệu theo ngữ cảnh kiểm thử | Không phân công driver và xử lý theo nhánh không tìm được driver. |

## UC-15 – Quản lý khách hàng
**Actor:** Nhân viên vận hành

| Test Case ID | Test Scenario | Preconditions | Test Steps | Test Data | Expected Result |
|---|---|---|---|---|---|
| TC-15-01 | Xem danh sách khách hàng | Admin/operator có quyền | Mở quản lý khách hàng | Dữ liệu theo ngữ cảnh kiểm thử | Danh sách khách hàng được hiển thị. |
| TC-15-02 | Tìm khách hàng có dữ liệu | Có khách hàng tồn tại | Nhập tiêu chí tìm kiếm hợp lệ | Dữ liệu theo ngữ cảnh kiểm thử | Hiển thị khách hàng phù hợp. |
| TC-15-03 | Tìm khách hàng không tồn tại | Có quyền quản lý | Nhập tiêu chí không khớp | Dữ liệu theo ngữ cảnh kiểm thử | Hiển thị không có kết quả. |
| TC-15-04 | Cập nhật khách hàng | Có quyền quản lý | Chọn khách hàng → sửa dữ liệu hợp lệ → Lưu | Dữ liệu theo ngữ cảnh kiểm thử | Thông tin được cập nhật. |
| TC-15-05 | Thao tác khi không có quyền | Tài khoản không có quyền quản lý | Truy cập chức năng | Dữ liệu theo ngữ cảnh kiểm thử | Hệ thống từ chối truy cập. |

## UC-16 – Quản lý tài xế & xe
**Actor:** Nhân viên vận hành

| Test Case ID | Test Scenario | Preconditions | Test Steps | Test Data | Expected Result |
|---|---|---|---|---|---|
| TC-16-01 | Xem danh sách tài xế và xe | Admin/operator có quyền | Mở chức năng | Dữ liệu theo ngữ cảnh kiểm thử | Danh sách được hiển thị. |
| TC-16-02 | Tìm tài xế/xe | Có dữ liệu | Nhập tiêu chí tìm kiếm | Dữ liệu theo ngữ cảnh kiểm thử | Kết quả phù hợp được hiển thị. |
| TC-16-03 | Cập nhật thông tin tài xế/xe | Có quyền | Sửa dữ liệu hợp lệ → Lưu | Dữ liệu theo ngữ cảnh kiểm thử | Thông tin được cập nhật. |
| TC-16-04 | Duyệt hồ sơ tài xế | Hồ sơ cần duyệt | Chọn hồ sơ → thực hiện duyệt | Dữ liệu theo ngữ cảnh kiểm thử | Trạng thái hồ sơ được cập nhật theo chức năng duyệt. |
| TC-16-05 | Từ chối thao tác khi không có quyền | Tài khoản không đủ quyền | Truy cập quản lý driver/vehicle | Dữ liệu theo ngữ cảnh kiểm thử | Hệ thống từ chối thao tác. |

## UC-17 – Giám sát chuyến đang diễn ra
**Actor:** Nhân viên vận hành

| Test Case ID | Test Scenario | Preconditions | Test Steps | Test Data | Expected Result |
|---|---|---|---|---|---|
| TC-17-01 | Xem danh sách chuyến đang diễn ra | Operator có quyền | Mở màn hình giám sát | Dữ liệu theo ngữ cảnh kiểm thử | Các chuyến đang diễn ra được hiển thị. |
| TC-17-02 | Xem thông tin chi tiết chuyến | Có active trip | Chọn một trip | Dữ liệu theo ngữ cảnh kiểm thử | Thông tin trip được hiển thị. |
| TC-17-03 | Giám sát cập nhật trạng thái | Trip đang hoạt động | Theo dõi trip khi trạng thái thay đổi | Dữ liệu theo ngữ cảnh kiểm thử | Thông tin giám sát phản ánh trạng thái mới. |
| TC-17-04 | Can thiệp khi có sự cố | Operator có quyền và có sự cố | Thực hiện thao tác can thiệp được SRS mô tả | Dữ liệu theo ngữ cảnh kiểm thử | Hệ thống thực hiện thao tác trong phạm vi được phép. |
| TC-17-05 | Truy cập khi không có quyền | Tài khoản không đủ quyền | Mở chức năng giám sát | Dữ liệu theo ngữ cảnh kiểm thử | Hệ thống từ chối truy cập. |

## UC-18 – Xem báo cáo
**Actor:** Nhân viên vận hành

| Test Case ID | Test Scenario | Preconditions | Test Steps | Test Data | Expected Result |
|---|---|---|---|---|---|
| TC-18-01 | Xem báo cáo theo khoảng thời gian hợp lệ | Operator có quyền; có dữ liệu | Chọn khoảng thời gian → xem báo cáo | Dữ liệu hợp lệ | Báo cáo tổng hợp dữ liệu Trip, Payment, Rating theo khoảng thời gian. |
| TC-18-02 | Khoảng thời gian không hợp lệ | Có quyền xem báo cáo | Nhập khoảng thời gian không hợp lệ | Dữ liệu hợp lệ | Hệ thống yêu cầu nhập lại hoặc từ chối điều kiện lọc. |
| TC-18-03 | Khoảng thời gian không có dữ liệu | Có quyền | Chọn khoảng thời gian không có giao dịch | Dữ liệu theo ngữ cảnh kiểm thử | Báo cáo hiển thị trạng thái không có dữ liệu. |
| TC-18-04 | Xem báo cáo khi không có quyền | Tài khoản không đủ quyền | Truy cập báo cáo | Dữ liệu theo ngữ cảnh kiểm thử | Hệ thống từ chối truy cập. |

## UC-19 – Phân quyền người dùng
**Actor:** Nhân viên vận hành cấp cao

| Test Case ID | Test Scenario | Preconditions | Test Steps | Test Data | Expected Result |
|---|---|---|---|---|---|
| TC-19-01 | Cấp quyền hợp lệ | Admin cấp cao đã đăng nhập | Chọn user/role → cấp quyền hợp lệ → Lưu | Dữ liệu hợp lệ | Quyền được cập nhật; người dùng có thể thực hiện chức năng tương ứng. |
| TC-19-02 | Thu hồi quyền | Admin cấp cao; user đang có quyền | Chọn user → thu hồi quyền → Lưu | Dữ liệu theo ngữ cảnh kiểm thử | Quyền được thu hồi. |
| TC-19-03 | Người không đủ quyền tự quản lý quyền | Tài khoản không phải admin cấp cao | Truy cập quản lý quyền | Dữ liệu theo ngữ cảnh kiểm thử | Hệ thống từ chối thao tác theo QT-21. |
| TC-19-04 | Truy cập chức năng sau khi quyền bị thu hồi | Quyền vừa bị thu hồi | User truy cập chức năng trước đó được phép | Dữ liệu theo ngữ cảnh kiểm thử | Hệ thống từ chối truy cập. |
| TC-19-05 | Ghi audit thao tác phân quyền | Admin cấp cao thực hiện thay đổi quyền | Cấp/thu hồi quyền → kiểm tra audit | Dữ liệu theo ngữ cảnh kiểm thử | Thao tác nhạy cảm được ghi audit log theo QT-22/BR-28. |

## 3. Tổng số Test Case

**Tổng cộng: 102 test case cho 19 Use Case.**

## 4. Lưu ý về các điểm chưa chốt trong SRS

- Công thức cước cụ thể của QT-10 chưa được chốt, vì vậy không kiểm thử bằng một con số cước tự đặt.
- Thời gian phản hồi của tài xế theo QT-03 chưa được chốt, vì vậy test timeout chỉ kiểm tra việc hệ thống xử lý nhánh timeout, không áp đặt số giây cụ thể.
- Số lần thử tối đa của Matching theo QT-05 chưa được chốt, vì vậy không tự đặt giá trị `[n]`.
- Điều kiện/phí hủy chuyến theo QT-14/QT-15 chưa được chốt đầy đủ; test chỉ kiểm tra các trạng thái được SRS mô tả.
- Notification không có UC riêng trong SRS; các test thông báo được đặt trong các UC tạo ra sự kiện tương ứng.
- NFR cần phương pháp kiểm thử riêng theo SRS, không gộp máy móc vào test case chức năng.
