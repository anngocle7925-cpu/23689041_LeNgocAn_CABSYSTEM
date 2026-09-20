# CAB Test Cases

> Bộ test case được xây dựng bám theo 22 Gherkin Scenario trong SRS CAB. Mỗi Test Scenario phát sinh tối đa 20 test case; không bổ sung các chức năng ngoài nội dung SRS/Acceptance Criteria.

> Tổng số test case: **77**.

## AC-03-01 - Đặt xe thành công

| Test Case ID | Test Case | Preconditions | Test Steps | Test Data | Expected Result | Priority |
|---|---|---|---|---|---|---|
| TC-BOOK-001 | Đặt xe với thông tin hợp lệ | Khách hàng đã đăng nhập; không có chuyến đang hoạt động | 1. Mở chức năng đặt xe<br>2. Nhập điểm đón và điểm đến hợp lệ<br>3. Xác nhận đặt xe | Điểm đón hợp lệ; điểm đến hợp lệ | Hệ thống tạo Trip mới với trạng thái “Đang tìm tài xế” và kích hoạt quy trình tìm tài xế. | High |
| TC-BOOK-002 | Tạo chuyến với điểm đón và điểm đến trong vùng phục vụ | Khách hàng đã đăng nhập; không có chuyến đang hoạt động | 1. Nhập điểm đón thuộc vùng phục vụ<br>2. Nhập điểm đến thuộc vùng phục vụ<br>3. Xác nhận đặt xe | Điểm đón/điểm đến trong vùng phục vụ | Yêu cầu đặt xe được chấp nhận và Trip được tạo. | High |
| TC-BOOK-003 | Kích hoạt tìm tài xế sau khi tạo chuyến | Trip vừa được tạo thành công | 1. Thực hiện đặt xe hợp lệ<br>2. Theo dõi trạng thái Trip | Trip trạng thái “Đang tìm tài xế” | Hệ thống kích hoạt chức năng tìm và gán tài xế. | High |
| TC-BOOK-004 | Kích hoạt tìm tài xế trong thời gian yêu cầu | Khách hàng đã đăng nhập; dữ liệu đặt xe hợp lệ | 1. Gửi yêu cầu đặt xe<br>2. Ghi nhận thời điểm tạo Trip<br>3. Kiểm tra thời điểm hệ thống kích hoạt tìm tài xế | Yêu cầu đặt xe hợp lệ | Quy trình tìm tài xế được kích hoạt trong tối đa 2 giây theo AC-03-01. | High |

## AC-03-02 - Từ chối đặt xe khi đã có chuyến đang hoạt động

| Test Case ID | Test Case | Preconditions | Test Steps | Test Data | Expected Result | Priority |
|---|---|---|---|---|---|---|
| TC-BOOK-005 | Từ chối đặt xe khi đang có chuyến “Đang tìm tài xế” | Khách hàng có một Trip đang hoạt động ở trạng thái “Đang tìm tài xế” | 1. Thực hiện đặt xe mới<br>2. Xác nhận yêu cầu | Trip hiện tại: “Đang tìm tài xế” | Hệ thống từ chối yêu cầu đặt xe mới; không tạo Trip mới. | High |
| TC-BOOK-006 | Từ chối đặt xe khi đang có chuyến đã có tài xế | Khách hàng có Trip đang hoạt động ở trạng thái “Đã có tài xế” | 1. Thực hiện đặt xe mới<br>2. Xác nhận yêu cầu | Trip hiện tại: “Đã có tài xế” | Hệ thống từ chối yêu cầu đặt xe mới; không tạo Trip mới. | High |
| TC-BOOK-007 | Từ chối đặt xe khi đang có chuyến đã đến điểm đón | Khách hàng có Trip đang hoạt động ở trạng thái “Đã đến điểm đón” | 1. Thực hiện đặt xe mới<br>2. Xác nhận yêu cầu | Trip hiện tại: “Đã đến điểm đón” | Hệ thống từ chối yêu cầu đặt xe mới; không tạo Trip mới. | High |
| TC-BOOK-008 | Từ chối đặt xe khi đang có chuyến đã đón khách | Khách hàng có Trip đang hoạt động ở trạng thái “Đã đón khách” | 1. Thực hiện đặt xe mới<br>2. Xác nhận yêu cầu | Trip hiện tại: “Đã đón khách” | Hệ thống từ chối yêu cầu đặt xe mới; không tạo Trip mới. | High |
| TC-BOOK-009 | Từ chối đặt xe khi đang có chuyến di chuyển | Khách hàng có Trip đang hoạt động ở trạng thái “Đang di chuyển” | 1. Thực hiện đặt xe mới<br>2. Xác nhận yêu cầu | Trip hiện tại: “Đang di chuyển” | Hệ thống từ chối yêu cầu đặt xe mới; không tạo Trip mới. | High |

## AC-03-03 - Điểm đón/điểm đến ngoài vùng phục vụ

| Test Case ID | Test Case | Preconditions | Test Steps | Test Data | Expected Result | Priority |
|---|---|---|---|---|---|---|
| TC-BOOK-010 | Từ chối khi điểm đón ngoài vùng phục vụ | Khách hàng đăng nhập; không có chuyến đang hoạt động | 1. Nhập điểm đón ngoài vùng phục vụ<br>2. Nhập điểm đến hợp lệ<br>3. Xác nhận đặt xe | Điểm đón ngoài vùng phục vụ | Hệ thống từ chối yêu cầu đặt xe. | High |
| TC-BOOK-011 | Từ chối khi điểm đến ngoài vùng phục vụ | Khách hàng đăng nhập; không có chuyến đang hoạt động | 1. Nhập điểm đón hợp lệ<br>2. Nhập điểm đến ngoài vùng phục vụ<br>3. Xác nhận đặt xe | Điểm đến ngoài vùng phục vụ | Hệ thống từ chối yêu cầu đặt xe. | High |
| TC-BOOK-012 | Từ chối khi cả điểm đón và điểm đến ngoài vùng phục vụ | Khách hàng đăng nhập; không có chuyến đang hoạt động | 1. Nhập điểm đón ngoài vùng phục vụ<br>2. Nhập điểm đến ngoài vùng phục vụ<br>3. Xác nhận đặt xe | Cả hai địa điểm ngoài vùng phục vụ | Hệ thống từ chối yêu cầu đặt xe; không tạo Trip. | High |

## AC-14-01 - Tìm thấy tài xế ngay lần đề xuất đầu tiên

| Test Case ID | Test Case | Preconditions | Test Steps | Test Data | Expected Result | Priority |
|---|---|---|---|---|---|---|
| TC-MATCH-001 | Tài xế đầu tiên chấp nhận chuyến | Trip ở trạng thái “Đang tìm tài xế”; có tài xế phù hợp | 1. Hệ thống đề xuất chuyến cho tài xế đầu tiên<br>2. Tài xế chấp nhận | Tài xế A: chấp nhận | Trip được gán cho tài xế A. | High |
| TC-MATCH-002 | Cập nhật tài xế vào Trip sau khi chấp nhận | Trip đang tìm tài xế; tài xế A được đề xuất | 1. Tài xế A chấp nhận<br>2. Kiểm tra thông tin Trip | Tài xế A: chấp nhận | Thông tin tài xế được gán vào Trip và trạng thái Trip được cập nhật phù hợp. | High |
| TC-MATCH-003 | Thông báo kết quả gán tài xế cho khách hàng | Trip được gán thành công | 1. Tài xế đầu tiên chấp nhận<br>2. Kiểm tra phía khách hàng | Tài xế A: chấp nhận | Khách hàng nhận được thông tin chuyến đã có tài xế. | High |

## AC-14-02 - Fallback khi tài xế từ chối

| Test Case ID | Test Case | Preconditions | Test Steps | Test Data | Expected Result | Priority |
|---|---|---|---|---|---|---|
| TC-MATCH-004 | Chuyển sang tài xế tiếp theo khi tài xế đầu tiên từ chối | Trip đang tìm tài xế; có ít nhất hai tài xế phù hợp | 1. Đề xuất cho tài xế A<br>2. Tài xế A từ chối<br>3. Theo dõi đề xuất tiếp theo | A: từ chối; B: sẵn sàng | Hệ thống chuyển sang đề xuất cho tài xế B. | High |
| TC-MATCH-005 | Không gán tài xế từ chối vào Trip | Trip đang tìm tài xế; tài xế A từ chối | 1. Tài xế A từ chối<br>2. Kiểm tra Trip | A: từ chối | Tài xế A không được gán vào Trip. | High |
| TC-MATCH-006 | Gán tài xế tiếp theo khi tài xế thứ hai chấp nhận | Trip đang tìm tài xế; A đã từ chối; B được đề xuất | 1. A từ chối<br>2. B chấp nhận<br>3. Kiểm tra Trip | A: từ chối; B: chấp nhận | Trip được gán cho tài xế B. | High |

## AC-14-03 - Fallback khi tài xế không phản hồi (timeout)

| Test Case ID | Test Case | Preconditions | Test Steps | Test Data | Expected Result | Priority |
|---|---|---|---|---|---|---|
| TC-MATCH-007 | Chuyển sang tài xế tiếp theo khi hết thời gian phản hồi | Trip đang tìm tài xế; tài xế A được đề xuất | 1. Đề xuất Trip cho A<br>2. Không phản hồi<br>3. Chờ hết thời gian phản hồi<br>4. Theo dõi đề xuất tiếp theo | A: không phản hồi | Sau khi hết thời gian phản hồi, hệ thống chuyển sang tài xế tiếp theo. | High |
| TC-MATCH-008 | Không gán tài xế không phản hồi | Trip đang tìm tài xế; A không phản hồi | 1. Đề xuất cho A<br>2. Để hết thời gian phản hồi<br>3. Kiểm tra Trip | A: timeout | A không được gán vào Trip. | High |
| TC-MATCH-009 | Gán tài xế tiếp theo sau timeout khi tài xế tiếp theo chấp nhận | Trip đang tìm tài xế; A timeout; B sẵn sàng | 1. A không phản hồi<br>2. Hệ thống chuyển sang B<br>3. B chấp nhận | A: timeout; B: chấp nhận | Trip được gán cho B. | High |

## AC-14-04 - Không tìm được tài xế sau số lần thử tối đa

| Test Case ID | Test Case | Preconditions | Test Steps | Test Data | Expected Result | Priority |
|---|---|---|---|---|---|---|
| TC-MATCH-010 | Kết thúc tìm kiếm khi các lần đề xuất đều không thành công | Trip đang tìm tài xế; các tài xế được đề xuất lần lượt từ chối hoặc timeout | 1. Thực hiện các lần đề xuất<br>2. Tất cả đều từ chối hoặc timeout<br>3. Theo dõi Trip | Các đề xuất đều thất bại | Hệ thống kết thúc quy trình tìm tài xế sau số lần thử tối đa theo cấu hình. | High |
| TC-MATCH-011 | Không tiếp tục đề xuất sau khi hết số lần thử | Trip đã hết số lần thử tìm tài xế | 1. Theo dõi Trip sau lần thử cuối<br>2. Kiểm tra các đề xuất tiếp theo | Đã đạt số lần thử tối đa | Hệ thống không tiếp tục gửi đề xuất tìm tài xế. | High |
| TC-MATCH-012 | Thông báo không tìm được tài xế | Trip không tìm được tài xế sau số lần thử tối đa | 1. Đợi kết thúc quy trình matching<br>2. Kiểm tra phía khách hàng | Không có tài xế nhận chuyến | Khách hàng được thông báo rằng hệ thống không tìm được tài xế. | High |

## AC-12-01 - Cập nhật trạng thái đúng thứ tự

| Test Case ID | Test Case | Preconditions | Test Steps | Test Data | Expected Result | Priority |
|---|---|---|---|---|---|---|
| TC-TRIP-001 | Cập nhật từ “Đã có tài xế” sang “Đã đến điểm đón” | Trip ở trạng thái “Đã có tài xế” | 1. Tài xế cập nhật trạng thái<br>2. Kiểm tra Trip | Đã có tài xế → Đã đến điểm đón | Hệ thống chấp nhận cập nhật và lưu trạng thái mới. | High |
| TC-TRIP-002 | Cập nhật từ “Đã đến điểm đón” sang “Đã đón khách” | Trip ở trạng thái “Đã đến điểm đón” | 1. Tài xế cập nhật trạng thái<br>2. Kiểm tra Trip | Đã đến điểm đón → Đã đón khách | Hệ thống chấp nhận cập nhật và lưu trạng thái mới. | High |
| TC-TRIP-003 | Cập nhật từ “Đã đón khách” sang “Đang di chuyển” | Trip ở trạng thái “Đã đón khách” | 1. Tài xế cập nhật trạng thái<br>2. Kiểm tra Trip | Đã đón khách → Đang di chuyển | Hệ thống chấp nhận cập nhật và lưu trạng thái mới. | High |
| TC-TRIP-004 | Gửi thông báo khi tài xế đã đến điểm đón | Trip được cập nhật sang “Đã đến điểm đón” | 1. Tài xế cập nhật trạng thái<br>2. Kiểm tra thông báo của khách hàng | Trạng thái: Đã đến điểm đón | Khách hàng nhận được thông báo tài xế đã đến điểm đón. | High |

## AC-12-02 - Từ chối cập nhật sai thứ tự

| Test Case ID | Test Case | Preconditions | Test Steps | Test Data | Expected Result | Priority |
|---|---|---|---|---|---|---|
| TC-TRIP-005 | Từ chối chuyển trực tiếp từ “Đã có tài xế” sang “Hoàn thành” | Trip ở trạng thái “Đã có tài xế” | 1. Tài xế yêu cầu hoàn thành Trip<br>2. Kiểm tra kết quả | Đã có tài xế → Hoàn thành | Hệ thống từ chối cập nhật; trạng thái Trip không thay đổi. | High |
| TC-TRIP-006 | Từ chối chuyển trực tiếp từ “Đã có tài xế” sang “Đang di chuyển” | Trip ở trạng thái “Đã có tài xế” | 1. Tài xế yêu cầu chuyển sang đang di chuyển<br>2. Kiểm tra kết quả | Đã có tài xế → Đang di chuyển | Hệ thống từ chối cập nhật; trạng thái Trip không thay đổi. | High |
| TC-TRIP-007 | Từ chối chuyển từ “Đã đến điểm đón” sang “Hoàn thành” | Trip ở trạng thái “Đã đến điểm đón” | 1. Tài xế yêu cầu hoàn thành Trip<br>2. Kiểm tra kết quả | Đã đến điểm đón → Hoàn thành | Hệ thống từ chối cập nhật; trạng thái Trip không thay đổi. | High |
| TC-TRIP-008 | Từ chối chuyển ngược trạng thái | Trip ở trạng thái “Đang di chuyển” | 1. Tài xế yêu cầu chuyển về trạng thái trước<br>2. Kiểm tra kết quả | Đang di chuyển → Đã đón khách | Hệ thống từ chối cập nhật; trạng thái Trip không thay đổi. | High |

## AC-12-03 - Hoàn thành chuyến và ghi nhận dữ liệu

| Test Case ID | Test Case | Preconditions | Test Steps | Test Data | Expected Result | Priority |
|---|---|---|---|---|---|---|
| TC-TRIP-009 | Hoàn thành chuyến từ trạng thái “Đang di chuyển” | Trip ở trạng thái “Đang di chuyển” | 1. Tài xế kết thúc chuyến<br>2. Kiểm tra trạng thái Trip | Đang di chuyển → Hoàn thành | Hệ thống chấp nhận và chuyển Trip sang “Hoàn thành”. | High |
| TC-TRIP-010 | Ghi nhận thời điểm hoàn thành | Trip ở trạng thái “Đang di chuyển” | 1. Hoàn thành chuyến<br>2. Kiểm tra dữ liệu Trip | Yêu cầu hoàn thành hợp lệ | Trường completed_at được ghi nhận. | High |
| TC-TRIP-011 | Tính khoảng cách chuyến đi | Trip ở trạng thái “Đang di chuyển”; có dữ liệu vị trí | 1. Hoàn thành chuyến<br>2. Kiểm tra dữ liệu khoảng cách | Dữ liệu vị trí trong chuyến | Hệ thống ghi nhận/tính distance_km của chuyến. | High |
| TC-TRIP-012 | Kích hoạt quy trình thanh toán sau khi hoàn thành | Trip được chuyển sang “Hoàn thành” | 1. Hoàn thành chuyến<br>2. Theo dõi bước tiếp theo | Trip: Hoàn thành | Hệ thống kích hoạt quy trình thanh toán. | High |

## AC-05-01 - Thanh toán tiền mặt thành công

| Test Case ID | Test Case | Preconditions | Test Steps | Test Data | Expected Result | Priority |
|---|---|---|---|---|---|---|
| TC-PAY-001 | Thanh toán tiền mặt thành công | Trip đã hoàn thành; phương thức thanh toán là tiền mặt | 1. Chọn/ghi nhận thanh toán tiền mặt<br>2. Xác nhận thanh toán | Phương thức: tiền mặt | Thanh toán được ghi nhận thành công. | High |
| TC-PAY-002 | Ghi nhận Trip đã thanh toán bằng tiền mặt | Trip đã hoàn thành; thanh toán tiền mặt thành công | 1. Thực hiện thanh toán tiền mặt<br>2. Kiểm tra trạng thái thanh toán | Tiền mặt: thành công | Trip được ghi nhận đã thanh toán. | High |
| TC-PAY-003 | Không yêu cầu cổng thanh toán điện tử khi chọn tiền mặt | Trip đã hoàn thành; phương thức tiền mặt | 1. Chọn phương thức tiền mặt<br>2. Thực hiện thanh toán<br>3. Kiểm tra luồng xử lý | Phương thức: tiền mặt | Hệ thống xử lý thanh toán tiền mặt mà không cần giao dịch điện tử. | High |

## AC-05-02 - Thanh toán điện tử thành công

| Test Case ID | Test Case | Preconditions | Test Steps | Test Data | Expected Result | Priority |
|---|---|---|---|---|---|---|
| TC-PAY-004 | Thanh toán điện tử thành công qua cổng thanh toán | Trip đã hoàn thành; phương thức e-payment; cổng thanh toán hoạt động | 1. Chọn thanh toán điện tử<br>2. Thực hiện thanh toán qua cổng<br>3. Kiểm tra kết quả | Gateway: thành công | Hệ thống ghi nhận thanh toán điện tử thành công. | High |
| TC-PAY-005 | Cập nhật trạng thái thanh toán sau giao dịch điện tử thành công | Giao dịch điện tử trả về kết quả thành công | 1. Nhận kết quả từ gateway<br>2. Kiểm tra Trip/thanh toán | Gateway result: success | Trip được ghi nhận đã thanh toán. | High |
| TC-PAY-006 | Thông báo kết quả thanh toán điện tử thành công | Giao dịch điện tử thành công | 1. Hoàn tất giao dịch<br>2. Kiểm tra phía khách hàng | Gateway result: success | Khách hàng nhận được kết quả thanh toán thành công. | High |

## AC-05-03 - Giao dịch điện tử thất bại

| Test Case ID | Test Case | Preconditions | Test Steps | Test Data | Expected Result | Priority |
|---|---|---|---|---|---|---|
| TC-PAY-007 | Xử lý giao dịch điện tử thất bại | Trip đã hoàn thành; phương thức e-payment | 1. Thực hiện thanh toán điện tử<br>2. Gateway trả về thất bại<br>3. Kiểm tra kết quả | Gateway result: failed | Hệ thống ghi nhận giao dịch thất bại và thông báo cho người dùng. | High |
| TC-PAY-008 | Cho phép thử lại khi thanh toán điện tử thất bại | Giao dịch điện tử thất bại | 1. Nhận thông báo thất bại<br>2. Chọn thử lại<br>3. Thực hiện lại thanh toán | Lần 1: failed; lần 2: success | Hệ thống cho phép thử lại và ghi nhận thành công nếu giao dịch thử lại thành công. | High |
| TC-PAY-009 | Cho phép chuyển sang tiền mặt khi thanh toán điện tử thất bại | Giao dịch điện tử thất bại | 1. Nhận thông báo thất bại<br>2. Chọn phương thức tiền mặt<br>3. Hoàn tất thanh toán tiền mặt | E-payment: failed; Cash: success | Hệ thống cho phép chuyển sang thanh toán tiền mặt và ghi nhận thanh toán thành công. | High |

## AC-05-04 - Không lưu thông tin nhạy cảm

| Test Case ID | Test Case | Preconditions | Test Steps | Test Data | Expected Result | Priority |
|---|---|---|---|---|---|---|
| TC-PAY-010 | Không lưu số thẻ trong cơ sở dữ liệu nội bộ | Hệ thống thanh toán điện tử hoạt động | 1. Thực hiện giao dịch điện tử<br>2. Kiểm tra dữ liệu thanh toán nội bộ | Thông tin thẻ được nhập tại cổng thanh toán | Cơ sở dữ liệu nội bộ không lưu số thẻ. | High |
| TC-PAY-011 | Không lưu thông tin tài khoản thanh toán trong cơ sở dữ liệu nội bộ | Hệ thống thanh toán điện tử hoạt động | 1. Thực hiện giao dịch điện tử<br>2. Kiểm tra dữ liệu nội bộ | Thông tin tài khoản thanh toán | Cơ sở dữ liệu nội bộ không lưu thông tin tài khoản thanh toán nhạy cảm. | High |
| TC-PAY-012 | Chỉ ghi nhận kết quả giao dịch cần thiết | Giao dịch điện tử hoàn tất | 1. Hoàn tất giao dịch<br>2. Kiểm tra bản ghi thanh toán nội bộ | Kết quả giao dịch từ gateway | Hệ thống chỉ ghi nhận dữ liệu phục vụ xử lý giao dịch, không lưu thông tin nhạy cảm của phương thức thanh toán. | High |

## AC-08-01 - Hủy thành công khi còn ở trạng thái cho phép

| Test Case ID | Test Case | Preconditions | Test Steps | Test Data | Expected Result | Priority |
|---|---|---|---|---|---|---|
| TC-CANCEL-001 | Hủy chuyến khi đang tìm tài xế | Trip ở trạng thái “Đang tìm tài xế” | 1. Chọn hủy chuyến<br>2. Xác nhận hủy<br>3. Kiểm tra Trip | Trip: Đang tìm tài xế | Trip được chuyển sang trạng thái hủy và ghi nhận lý do hủy. | Medium |
| TC-CANCEL-002 | Hủy chuyến khi đã có tài xế | Trip ở trạng thái “Đã có tài xế” | 1. Chọn hủy chuyến<br>2. Xác nhận hủy<br>3. Kiểm tra Trip và thông báo | Trip: Đã có tài xế | Trip được hủy; lý do được ghi nhận và tài xế được thông báo. | Medium |
| TC-CANCEL-003 | Ghi nhận lý do hủy chuyến | Trip ở trạng thái cho phép hủy | 1. Chọn hủy<br>2. Nhập/chọn lý do<br>3. Xác nhận | Lý do hủy hợp lệ | Lý do hủy được lưu cùng thông tin Trip. | Medium |
| TC-CANCEL-004 | Đưa tài xế về trạng thái sẵn sàng sau khi chuyến bị hủy | Trip đã có tài xế và được hủy | 1. Hủy Trip<br>2. Kiểm tra trạng thái của tài xế | Trip: Đã có tài xế → Hủy | Tài xế được đưa về trạng thái phù hợp để có thể nhận chuyến tiếp theo. | Medium |

## AC-08-02 - Từ chối hủy khi chuyến đã bắt đầu di chuyển

| Test Case ID | Test Case | Preconditions | Test Steps | Test Data | Expected Result | Priority |
|---|---|---|---|---|---|---|
| TC-CANCEL-005 | Từ chối hủy khi Trip đang di chuyển | Trip ở trạng thái “Đang di chuyển” | 1. Khách hàng yêu cầu hủy<br>2. Xác nhận<br>3. Kiểm tra kết quả | Trip: Đang di chuyển | Hệ thống từ chối yêu cầu hủy; Trip vẫn ở trạng thái hiện tại. | Medium |
| TC-CANCEL-006 | Không thay đổi trạng thái Trip khi hủy bị từ chối | Trip đang ở “Đang di chuyển” | 1. Gửi yêu cầu hủy<br>2. Kiểm tra trạng thái trước và sau | Trip: Đang di chuyển | Trạng thái Trip không thay đổi. | Medium |
| TC-CANCEL-007 | Từ chối hủy sau khi chuyến đã hoàn thành | Trip ở trạng thái “Hoàn thành” | 1. Thực hiện yêu cầu hủy<br>2. Kiểm tra kết quả | Trip: Hoàn thành | Hệ thống không cho phép hủy Trip đã hoàn thành. | Medium |

## AC-07-01 - Đánh giá thành công sau khi hoàn thành

| Test Case ID | Test Case | Preconditions | Test Steps | Test Data | Expected Result | Priority |
|---|---|---|---|---|---|---|
| TC-RATING-001 | Đánh giá tài xế sau khi hoàn thành chuyến | Trip đã hoàn thành; chưa được đánh giá | 1. Mở chức năng đánh giá<br>2. Chọn mức đánh giá<br>3. Gửi đánh giá | Trip: Hoàn thành; chưa đánh giá | Hệ thống lưu đánh giá thành công. | Medium |
| TC-RATING-002 | Chỉ cho phép đánh giá sau khi Trip hoàn thành | Trip đã hoàn thành | 1. Mở chức năng đánh giá<br>2. Gửi mức đánh giá | Trip: Hoàn thành | Hệ thống cho phép ghi nhận đánh giá. | Medium |
| TC-RATING-003 | Cập nhật điểm đánh giá trung bình của tài xế | Đánh giá được gửi thành công | 1. Gửi đánh giá<br>2. Kiểm tra thông tin đánh giá của tài xế | Mức đánh giá hợp lệ | Điểm đánh giá trung bình của tài xế được cập nhật theo dữ liệu đánh giá. | Medium |

## AC-07-02 - Không thể đánh giá hai lần

| Test Case ID | Test Case | Preconditions | Test Steps | Test Data | Expected Result | Priority |
|---|---|---|---|---|---|---|
| TC-RATING-004 | Từ chối đánh giá lần thứ hai cho cùng Trip | Trip đã hoàn thành và đã được đánh giá | 1. Mở lại chức năng đánh giá<br>2. Gửi một đánh giá mới | Trip đã có đánh giá | Hệ thống từ chối đánh giá lần thứ hai. | Medium |
| TC-RATING-005 | Không tạo thêm bản ghi đánh giá khi gửi lần hai | Trip đã có một đánh giá | 1. Gửi đánh giá lần hai<br>2. Kiểm tra dữ liệu đánh giá | Trip đã được đánh giá | Không phát sinh thêm bản ghi đánh giá cho cùng Trip. | Medium |
| TC-RATING-006 | Không thay đổi điểm do đánh giá trùng | Trip đã có một đánh giá | 1. Ghi nhận điểm hiện tại<br>2. Thử đánh giá lần hai<br>3. Kiểm tra lại điểm | Trip đã được đánh giá | Đánh giá bị từ chối và điểm đánh giá không bị thay đổi bởi lần gửi trùng. | Medium |

## AC-13-01 - Ghi nhận vị trí định kỳ trong chuyến

| Test Case ID | Test Case | Preconditions | Test Steps | Test Data | Expected Result | Priority |
|---|---|---|---|---|---|---|
| TC-LOC-001 | Ghi nhận vị trí tài xế trong chuyến | Trip đang ở trạng thái “Đang di chuyển”; tài xế đang gửi vị trí | 1. Gửi tọa độ vị trí<br>2. Kiểm tra dữ liệu Trip/location | Tọa độ hợp lệ | Hệ thống ghi nhận vị trí của tài xế. | Medium |
| TC-LOC-002 | Ghi nhận nhiều vị trí định kỳ trong cùng chuyến | Trip đang di chuyển | 1. Gửi nhiều cập nhật vị trí theo các thời điểm khác nhau<br>2. Kiểm tra lịch sử vị trí | Nhiều tọa độ theo thời gian | Các cập nhật vị trí được ghi nhận định kỳ trong chuyến. | Medium |
| TC-LOC-003 | Cập nhật vị trí gần thời gian thực cho khách hàng | Trip đang di chuyển; khách hàng đang theo dõi chuyến | 1. Tài xế gửi cập nhật vị trí<br>2. Kiểm tra bản đồ phía khách hàng | Tọa độ mới của tài xế | Vị trí tài xế được cập nhật trên bản đồ của khách hàng theo cơ chế gần thời gian thực. | Medium |

## AC-19-01 - Phân quyền đúng vai trò

| Test Case ID | Test Case | Preconditions | Test Steps | Test Data | Expected Result | Priority |
|---|---|---|---|---|---|---|
| TC-ADMIN-001 | Cho phép người dùng có vai trò phù hợp truy cập chức năng được cấp quyền | Người dùng đã đăng nhập với vai trò được cấp quyền | 1. Đăng nhập<br>2. Truy cập chức năng được cấp quyền | Vai trò có quyền truy cập | Hệ thống cho phép truy cập chức năng theo quyền của vai trò. | Medium |
| TC-ADMIN-002 | Từ chối nhân viên hỗ trợ truy cập chức năng chỉ dành cho quản trị | Người dùng đăng nhập với vai trò nhân viên hỗ trợ | 1. Đăng nhập<br>2. Truy cập chức năng quản trị bị giới hạn | Vai trò: Support; chức năng: quản trị | Hệ thống từ chối truy cập. | Medium |
| TC-ADMIN-003 | Không cho phép thực hiện thao tác khi không có quyền | Người dùng đã đăng nhập nhưng không có quyền thao tác | 1. Truy cập chức năng<br>2. Thực hiện thao tác | Vai trò không có quyền | Hệ thống từ chối thao tác. | Medium |
| TC-ADMIN-004 | Phân quyền được áp dụng theo vai trò | Các vai trò được cấu hình trong hệ thống | 1. Đăng nhập bằng từng vai trò<br>2. Kiểm tra quyền truy cập tương ứng | Các vai trò trong hệ thống | Mỗi vai trò chỉ được thực hiện các chức năng được cấp quyền. | Medium |

## AC-18-01 - Báo cáo chính xác theo khoảng thời gian

| Test Case ID | Test Case | Preconditions | Test Steps | Test Data | Expected Result | Priority |
|---|---|---|---|---|---|---|
| TC-REPORT-001 | Lọc báo cáo theo khoảng thời gian | Có dữ liệu chuyến trong hệ thống | 1. Mở báo cáo<br>2. Chọn khoảng thời gian<br>3. Xem báo cáo | Khoảng thời gian hợp lệ | Báo cáo chỉ tổng hợp dữ liệu thuộc khoảng thời gian đã chọn. | Medium |
| TC-REPORT-002 | Đối chiếu tổng số chuyến trong báo cáo | Có dữ liệu Trip trong khoảng thời gian chọn | 1. Chọn khoảng thời gian<br>2. Xem tổng số chuyến<br>3. Đối chiếu với dữ liệu Trip | Khoảng thời gian hợp lệ | Tổng số chuyến trong báo cáo khớp với dữ liệu nguồn. | Medium |
| TC-REPORT-003 | Đối chiếu doanh thu trong báo cáo | Có dữ liệu thanh toán trong khoảng thời gian chọn | 1. Chọn khoảng thời gian<br>2. Xem doanh thu<br>3. Đối chiếu dữ liệu thanh toán | Khoảng thời gian hợp lệ | Doanh thu trong báo cáo khớp với dữ liệu nguồn. | Medium |
| TC-REPORT-004 | Đối chiếu tỷ lệ hoàn thành và hủy chuyến | Có dữ liệu Trip với các trạng thái khác nhau | 1. Chọn khoảng thời gian<br>2. Xem tỷ lệ hoàn thành/hủy<br>3. Đối chiếu dữ liệu nguồn | Khoảng thời gian hợp lệ | Các tỷ lệ trong báo cáo được tính chính xác theo dữ liệu trong khoảng thời gian. | Medium |
| TC-REPORT-005 | Báo cáo phản ánh hiệu suất tài xế | Có dữ liệu hoạt động của tài xế trong khoảng thời gian | 1. Chọn khoảng thời gian<br>2. Xem phần hiệu suất tài xế<br>3. Đối chiếu dữ liệu nguồn | Khoảng thời gian hợp lệ | Thông tin hiệu suất tài xế trong báo cáo khớp với dữ liệu hệ thống. | Medium |

## AC-22-01 - Ghi audit log cho thao tác nhạy cảm

| Test Case ID | Test Case | Preconditions | Test Steps | Test Data | Expected Result | Priority |
|---|---|---|---|---|---|---|
| TC-AUDIT-001 | Ghi audit log khi khóa tài khoản tài xế | Người vận hành có quyền; tài xế tồn tại | 1. Thực hiện khóa tài khoản tài xế<br>2. Kiểm tra audit log | Đối tượng: tài xế; thao tác: khóa tài khoản | Hệ thống tạo audit log cho thao tác nhạy cảm. | Medium |
| TC-AUDIT-002 | Audit log ghi nhận người thực hiện | Thao tác khóa tài khoản đã thực hiện | 1. Thực hiện thao tác<br>2. Kiểm tra log | Operator thực hiện thao tác | Audit log ghi nhận đúng người thực hiện. | Medium |
| TC-AUDIT-003 | Audit log ghi nhận hành động và đối tượng | Thao tác khóa tài khoản đã thực hiện | 1. Kiểm tra bản ghi audit | Action: khóa; Object: tài khoản tài xế | Log thể hiện hành động và đối tượng bị tác động. | Medium |
| TC-AUDIT-004 | Audit log ghi nhận thời điểm thao tác | Thao tác khóa tài khoản đã thực hiện | 1. Thực hiện thao tác<br>2. Kiểm tra thời gian trong log | Thời điểm thực hiện thao tác | Audit log ghi nhận thời điểm thực hiện thao tác. | Medium |
