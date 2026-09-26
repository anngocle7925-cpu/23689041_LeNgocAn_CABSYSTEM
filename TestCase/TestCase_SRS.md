# Test Case (theo SRS) — CAB System

Bộ test case này được sinh **trực tiếp từ `srs.md`** (Bước 1 nghiệp vụ, Bước 6 Functional Requirements, Bước 7 Business Rules, Bước 13 Acceptance Criteria), viết theo **góc nhìn người dùng cuối sử dụng app** — không tham chiếu `openapi.yaml`, không có HTTP method/JSON/status code.

Đây là bộ test case **song song** với [`TestCase.md`](../TestCase/TestCase.md) (bộ test case theo API, xem trực tiếp trong `openapi.yaml`). Hai bộ phục vụ 2 mục đích khác nhau:

| | `TestCase.md` (theo API) | `TestCase_SRS.md` (theo SRS, tài liệu này) |
|---|---|---|
| Góc nhìn | Gọi trực tiếp API (Postman/code) | Thao tác trên giao diện như người dùng thật |
| Kiểm tra | API có đúng theo `openapi.yaml` không (request/response, mã lỗi HTTP) | Hệ thống có đúng theo nghiệp vụ trong `srs.md` không (luồng, thông báo, trải nghiệm) |
| Phạm vi | Bao gồm cả API nội bộ (matching, webhook) | Chỉ các luồng có thao tác người dùng thực tế |

File Excel gốc: [`CAB_Test_Cases_SRS.xlsx`](./CAB_Test_Cases_SRS.xlsx).

## Chú giải các cột

| Cột | Ý nghĩa | Mô tả chi tiết | Ví dụ |
|---|---|---|---|
| Test Case ID | Mã định danh duy nhất của Test Case | Dùng để quản lý, tìm kiếm, truy xuất và tham chiếu Test Case. | TC_LOGIN_001 |
| Test Scenario | Scenario mà Test Case đang kiểm thử | Mô tả chức năng hoặc tình huống cần kiểm thử ở mức tổng quát. Một Test Scenario có thể có nhiều Test Case. | Kiểm tra chức năng đăng nhập |
| Test Case | Trường hợp kiểm thử cụ thể | Mô tả chính xác trường hợp cần kiểm thử được phân rã từ Test Scenario. | Đăng nhập với username và password hợp lệ |
| Preconditions | Điều kiện tiên quyết | Các điều kiện hoặc trạng thái phải được đáp ứng trước khi bắt đầu thực hiện Test Case. | User đã đăng ký tài khoản và đang ở màn hình Login |
| Test Steps | Các bước thực hiện kiểm thử | Mô tả tuần tự các thao tác mà Tester cần thực hiện để kiểm tra Test Case. | 1. Mở Login<br>2. Nhập username<br>3. Nhập password<br>4. Nhấn Login |
| Test Data | Dữ liệu kiểm thử | Dữ liệu đầu vào cụ thể được sử dụng trong quá trình thực hiện Test Case. | Username: user01<br>Password: 123456 |
| Expected Result | Kết quả mong đợi | Kết quả mà hệ thống phải trả về nếu chức năng hoạt động đúng. Dùng để so sánh với Actual Result. | Đăng nhập thành công và chuyển đến trang Home |
| Priority | Mức độ ưu tiên | Cho biết mức độ quan trọng của Test Case và thứ tự ưu tiên khi thực hiện kiểm thử. | High / Medium / Low |

## Phương pháp thiết kế test case

Áp dụng đủ 5 tiêu chí cho mỗi luồng nghiệp vụ:

1. **Positive** — thao tác đúng, luồng thành công
2. **Negative** — thao tác/dữ liệu sai, kiểm tra app từ chối đúng cách
3. **Boundary / Power** — giá trị biên, bất thường (chuỗi rất dài, ký tự đặc biệt, mất mạng...)
4. **Bỏ trống / thiếu thao tác** — không nhập gì, bỏ qua bước bắt buộc
5. **Format** — sai định dạng hiển thị (ngày tháng, số điện thoại, đơn vị tiền tệ...)

Test case đánh dấu **`[CẦN LÀM RÕ]`** ứng với các điểm `srs.md` **chưa chốt** quy tắc nghiệp vụ (công thức cước, chính sách hủy chuyến, brute-force login...) — không tự bịa kết quả mong đợi, ghi rõ cần BA/khách hàng xác nhận trước.

## Tổng quan bộ test case

| # | Sheet / Nghiệp vụ | Số test case | Nguồn trong srs.md |
|---|---|---|---|
| 1 | Đăng nhập | 15 | Bước 1 (actor Khách hàng/Tài xế), Bước 6 FR-02 |
| 2 | Đăng ký tài khoản | 11 | Bước 6 FR-01, FR-04 |
| 3 | Hồ sơ cá nhân | 13 | Bước 6 FR-03, FR-05; Bước 7 QT-07 |
| 4 | Đặt xe & tìm tài xế | 15 | Bước 6 FR-07→10, FR-12, FR-14→17; Bước 7 QT-01→06 |
| 5 | Hủy & cập nhật trạng thái | 12 | Bước 6 FR-11, FR-21; Bước 7 QT-09, QT-14 |
| 6 | Đánh giá tài xế | 10 | Bước 6 FR-13; Bước 7 QT-17, QT-18 |
| 7 | Vận hành tài xế | 11 | Bước 6 FR-18→20, FR-22; Bước 7 QT-03, QT-04, QT-08 |
| 8 | Thanh toán | 10 | Bước 6 FR-23→28; Bước 7 QT-10→13 |
| 9 | Thông báo | 8 | Bước 6 FR-29, FR-30; Bước 7 QT-19, QT-20 |
| 10 | Quản trị vận hành | 14 | Bước 6 FR-32→37; Bước 7 QT-21, QT-22 |
| | **Tổng cộng** | **119** | |

## Đăng nhập

| Test Case ID | Test Scenario | Test Case | Preconditions | Test Steps | Test Data | Expected Result | Priority |
|---|---|---|---|---|---|---|---|
| TC-LOGIN-001 | Đăng nhập | Đăng nhập thành công với tài khoản khách hàng hợp lệ | Đã có tài khoản khách hàng đang hoạt động (active) | 1. Mở app CAB<br>2. Nhập số điện thoại đã đăng ký<br>3. Nhập đúng mật khẩu<br>4. Nhấn "Đăng nhập" | SĐT: 0901234567<br>Mật khẩu: Passw0rd@123 | Đăng nhập thành công, chuyển vào màn hình chính của khách hàng | High |
| TC-LOGIN-002 | Đăng nhập | Đăng nhập thành công với tài khoản tài xế hợp lệ | Đã có tài khoản tài xế đang hoạt động, chưa bị khóa | 1. Mở app Tài xế<br>2. Nhập SĐT và mật khẩu đúng<br>3. Nhấn "Đăng nhập" | SĐT: 0912345678<br>Mật khẩu: DriverPass@123 | Đăng nhập thành công, vào màn hình chính của tài xế | High |
| TC-LOGIN-003 | Đăng nhập | Đăng nhập với số điện thoại chưa từng đăng ký | SĐT chưa tồn tại trong hệ thống | 1. Nhập SĐT lạ<br>2. Nhập mật khẩu bất kỳ<br>3. Nhấn Đăng nhập | SĐT: 0900000000<br>Mật khẩu: Anything@123 | Hiển thị thông báo lỗi chung (không nói rõ sai SĐT hay sai mật khẩu, để tránh dò tài khoản) | High |
| TC-LOGIN-004 | Đăng nhập | Đăng nhập với mật khẩu sai | Tài khoản tồn tại, đang hoạt động | 1. Nhập đúng SĐT<br>2. Nhập sai mật khẩu<br>3. Nhấn Đăng nhập | SĐT: 0901234567<br>Mật khẩu: SaiRoi@999 | Hiển thị thông báo lỗi đăng nhập, không cho vào hệ thống | High |
| TC-LOGIN-005 | Đăng nhập | Không nhập số điện thoại | Đang ở màn hình đăng nhập | 1. Để trống ô SĐT<br>2. Nhập mật khẩu<br>3. Nhấn Đăng nhập | SĐT: (để trống)<br>Mật khẩu: Passw0rd@123 | App báo lỗi ngay tại chỗ, yêu cầu nhập số điện thoại, không cho gửi request | High |
| TC-LOGIN-006 | Đăng nhập | Không nhập mật khẩu | Đang ở màn hình đăng nhập | 1. Nhập SĐT<br>2. Để trống mật khẩu<br>3. Nhấn Đăng nhập | SĐT: 0901234567<br>Mật khẩu: (để trống) | App báo lỗi yêu cầu nhập mật khẩu | High |
| TC-LOGIN-007 | Đăng nhập | Không nhập gì cả, nhấn Đăng nhập luôn | Đang ở màn hình đăng nhập | 1. Không nhập gì<br>2. Nhấn Đăng nhập | (để trống cả 2 ô) | App báo lỗi cho cả 2 trường cùng lúc | High |
| TC-LOGIN-008 | Đăng nhập | Nhập số điện thoại sai định dạng (có chữ cái) | Đang ở màn hình đăng nhập | 1. Nhập SĐT có lẫn chữ<br>2. Nhập mật khẩu<br>3. Nhấn Đăng nhập | SĐT: 09A1234567<br>Mật khẩu: Passw0rd@123 | App báo SĐT không đúng định dạng, không cho gửi request lên server | Medium |
| TC-LOGIN-009 | Đăng nhập | Nhập số điện thoại quá ngắn | Đang ở màn hình đăng nhập | 1. Nhập SĐT chỉ 4 số<br>2. Nhấn Đăng nhập | SĐT: 0901 | App báo số điện thoại không hợp lệ | Medium |
| TC-LOGIN-010 | Đăng nhập | [CẦN LÀM RÕ] Đăng nhập bằng số điện thoại dạng +84 thay vì đầu số 0 | Tài khoản đã đăng ký bằng SĐT dạng 0901234567 | 1. Nhập SĐT dạng +84901234567<br>2. Nhập đúng mật khẩu<br>3. Nhấn Đăng nhập | SĐT: +84901234567<br>Mật khẩu: Passw0rd@123 | SRS chưa quy định cách xử lý 2 định dạng số điện thoại. Kỳ vọng tối thiểu: hệ thống nhận diện đây là cùng 1 tài khoản và đăng nhập được — cần BA xác nhận trước khi chốt | Medium (cần làm rõ) |
| TC-LOGIN-011 | Đăng nhập | Mật khẩu có khoảng trắng thừa ở đầu/cuối do gõ nhầm | Mật khẩu gốc không có khoảng trắng | 1. Nhập đúng SĐT<br>2. Gõ mật khẩu nhưng lỡ tay có dấu cách đầu/cuối<br>3. Nhấn Đăng nhập | Mật khẩu: " Passw0rd@123 " | Đăng nhập thất bại vì hệ thống coi khoảng trắng là 1 phần của mật khẩu — không tự động lược bỏ, tránh sai lệch bảo mật | Medium |
| TC-LOGIN-012 | Đăng nhập | Đăng nhập bằng tài khoản tài xế đang bị khóa | Tài khoản tài xế đã bị nhân viên vận hành khóa | 1. Nhập đúng SĐT/mật khẩu của tài xế đã bị khóa<br>2. Nhấn Đăng nhập | SĐT/Mật khẩu đúng của tài xế đã khóa | App hiển thị thông báo tài khoản đã bị khóa/vô hiệu hóa, không cho vào hệ thống | High |
| TC-LOGIN-013 | Đăng nhập | Thử đăng nhập với ký tự đặc biệt/mã độc trong ô nhập liệu | Đang ở màn hình đăng nhập | 1. Nhập SĐT/mật khẩu chứa ký tự đặc biệt kiểu tấn công | SĐT: 0901234567' OR '1'='1 | Đăng nhập thất bại như bình thường, hệ thống không bị lỗi/crash, không bị vượt qua xác thực | High |
| TC-LOGIN-014 | Đăng nhập | Sau khi đăng nhập thành công, có thể sử dụng các chức năng cần đăng nhập | Vừa đăng nhập thành công | 1. Đăng nhập thành công<br>2. Vào màn hình "Hồ sơ của tôi" | (dùng phiên đăng nhập vừa tạo) | Xem được đúng thông tin hồ sơ của chính tài khoản vừa đăng nhập, không bị văng ra lại màn hình đăng nhập | High |
| TC-LOGIN-015 | Đăng nhập | [CẦN LÀM RÕ] Đăng nhập sai liên tục nhiều lần | SRS chưa định nghĩa cơ chế khóa tài khoản khi đăng nhập sai nhiều lần | 1. Nhập sai mật khẩu 10 lần liên tiếp cho cùng 1 tài khoản | Mật khẩu sai lặp lại 10 lần | CHƯA có kết quả mong đợi chính thức — đây là khoảng trống bảo mật cần đề xuất bổ sung quy tắc mới, không tự giả định (vd không mặc định khóa sau 5 lần) khi chưa có xác nhận từ BA | Medium (cần làm rõ) |

## Đăng ký tài khoản

| Test Case ID | Test Scenario | Test Case | Preconditions | Test Steps | Test Data | Expected Result | Priority |
|---|---|---|---|---|---|---|---|
| TC-REG-001 | Đăng ký tài khoản | Khách hàng đăng ký tài khoản mới thành công | Chưa có tài khoản với SĐT này | 1. Mở app, chọn "Đăng ký"<br>2. Nhập họ tên, SĐT, mật khẩu<br>3. Nhấn "Đăng ký" | Họ tên: Nguyễn Văn A<br>SĐT: 0987000001<br>Mật khẩu: Passw0rd@123 | Đăng ký thành công, tự động đăng nhập vào hệ thống | High |
| TC-REG-002 | Đăng ký tài khoản | Tài xế đăng ký tài khoản mới thành công | Chưa có tài khoản với SĐT này | 1. Mở app Tài xế, chọn "Đăng ký"<br>2. Nhập họ tên, SĐT, mật khẩu<br>3. Nhấn "Đăng ký" | Họ tên: Trần Văn B<br>SĐT: 0987000002<br>Mật khẩu: Passw0rd@123 | Đăng ký thành công, chuyển sang bước hoàn thiện hồ sơ/phương tiện | High |
| TC-REG-003 | Đăng ký tài khoản | Đăng ký khách hàng với SĐT đã tồn tại | SĐT 0987000001 đã đăng ký ở TC-REG-001 | 1. Đăng ký lại với cùng SĐT | SĐT: 0987000001 | App báo SĐT đã được sử dụng, yêu cầu đăng nhập hoặc dùng SĐT khác | High |
| TC-REG-004 | Đăng ký tài khoản | Không nhập số điện thoại khi đăng ký | Đang ở màn hình đăng ký | 1. Bỏ trống SĐT<br>2. Nhập các trường còn lại<br>3. Nhấn Đăng ký | SĐT: (để trống) | App báo lỗi yêu cầu nhập SĐT | High |
| TC-REG-005 | Đăng ký tài khoản | Không nhập mật khẩu khi đăng ký | Đang ở màn hình đăng ký | 1. Bỏ trống mật khẩu<br>2. Nhấn Đăng ký | Mật khẩu: (để trống) | App báo lỗi yêu cầu nhập mật khẩu | High |
| TC-REG-006 | Đăng ký tài khoản | Không nhập họ tên khi đăng ký | Đang ở màn hình đăng ký | 1. Bỏ trống họ tên<br>2. Nhấn Đăng ký | Họ tên: (để trống) | App báo lỗi yêu cầu nhập họ tên | High |
| TC-REG-007 | Đăng ký tài khoản | Nhập email sai định dạng | Đang ở màn hình đăng ký (email là tùy chọn) | 1. Nhập email sai định dạng<br>2. Nhấn Đăng ký | Email: abc-khong-hop-le | App báo email không đúng định dạng | Medium |
| TC-REG-008 | Đăng ký tài khoản | [CẦN LÀM RÕ] Mật khẩu quá ngắn (dưới 5 ký tự) | SRS chưa quy định độ dài tối thiểu mật khẩu | 1. Nhập mật khẩu ngắn<br>2. Nhấn Đăng ký | Mật khẩu: 123 | Chưa có quy tắc chính thức. Đề xuất bổ sung yêu cầu tối thiểu 8 ký tự — cần BA xác nhận trước khi chốt kết quả mong đợi | Medium (cần làm rõ) |
| TC-REG-009 | Đăng ký tài khoản | Họ tên có dấu tiếng Việt | Đang ở màn hình đăng ký | 1. Nhập họ tên có dấu<br>2. Nhấn Đăng ký | Họ tên: Nguyễn Thị Hương | Đăng ký thành công, họ tên hiển thị đúng dấu ở các màn hình sau | Medium |
| TC-REG-010 | Đăng ký tài khoản | Sau khi đăng ký, hồ sơ không hiển thị lộ mật khẩu ở bất kỳ đâu | Vừa đăng ký thành công | 1. Vào lại màn hình hồ sơ ngay sau khi đăng ký | (dùng tài khoản vừa tạo) | Màn hình hồ sơ không hiển thị mật khẩu dưới bất kỳ hình thức nào (kể cả dạng ẩn ký tự có thể export) | High |
| TC-REG-011 | Đăng ký tài khoản | [CẦN LÀM RÕ] Một SĐT vừa là khách hàng vừa muốn đăng ký thêm tài khoản tài xế | SĐT đã là tài khoản khách hàng | 1. Dùng cùng SĐT đăng ký tài khoản tài xế | SĐT: 0987000001 (đã là khách hàng) | SRS chưa quy định 1 SĐT có được giữ 2 vai trò. Đề xuất từ chối để tránh nhầm lẫn — cần BA xác nhận | Medium (cần làm rõ) |

## Hồ sơ cá nhân

| Test Case ID | Test Scenario | Test Case | Preconditions | Test Steps | Test Data | Expected Result | Priority |
|---|---|---|---|---|---|---|---|
| TC-PROF-001 | Hồ sơ cá nhân | Khách hàng xem hồ sơ của mình | Đã đăng nhập | 1. Vào mục "Hồ sơ của tôi" | (không cần nhập gì) | Hiển thị đúng họ tên, ảnh đại diện, điểm đánh giá trung bình của khách hàng | High |
| TC-PROF-002 | Hồ sơ cá nhân | Khách hàng cập nhật họ tên | Đã đăng nhập | 1. Vào Hồ sơ<br>2. Sửa họ tên<br>3. Nhấn Lưu | Họ tên mới: Nguyễn Văn A2 | Hồ sơ được cập nhật, họ tên mới hiển thị ngay | High |
| TC-PROF-003 | Hồ sơ cá nhân | Khách hàng cập nhật ảnh đại diện | Đã đăng nhập | 1. Vào Hồ sơ<br>2. Chọn ảnh mới<br>3. Nhấn Lưu | Chọn 1 ảnh từ thư viện máy | Ảnh đại diện được cập nhật và hiển thị đúng ở các màn hình khác (vd. tài xế nhìn thấy ảnh khi nhận chuyến) | Medium |
| TC-PROF-004 | Hồ sơ cá nhân | Nhập email sai định dạng khi cập nhật hồ sơ | Đã đăng nhập | 1. Sửa email thành chuỗi không hợp lệ<br>2. Nhấn Lưu | Email: khong-hop-le | App báo lỗi định dạng email, không lưu được | Medium |
| TC-PROF-005 | Hồ sơ cá nhân | Không sửa gì, chỉ nhấn Lưu | Đã đăng nhập | 1. Vào Hồ sơ<br>2. Không sửa gì<br>3. Nhấn Lưu | (không thay đổi gì) | Hồ sơ giữ nguyên, không báo lỗi | Low |
| TC-PROF-006 | Hồ sơ cá nhân | Khách hàng yêu cầu xóa tài khoản khi không có chuyến đang chạy | Không có chuyến nào đang thực hiện | 1. Vào Hồ sơ<br>2. Chọn "Xóa tài khoản"<br>3. Xác nhận | (xác nhận xóa) | Tài khoản bị vô hiệu hóa, đăng xuất khỏi app, không đăng nhập lại được nữa | High |
| TC-PROF-007 | Hồ sơ cá nhân | Khách hàng yêu cầu xóa tài khoản khi đang có chuyến chưa xong | Đang có 1 chuyến ở trạng thái đang tìm tài xế/đang di chuyển | 1. Vào Hồ sơ<br>2. Chọn "Xóa tài khoản" | (xác nhận xóa) | App từ chối, thông báo cần hoàn thành/hủy chuyến hiện tại trước khi xóa tài khoản | High |
| TC-PROF-008 | Hồ sơ cá nhân | Tài xế xem hồ sơ và thông tin xe của mình | Đã đăng nhập bằng tài khoản tài xế | 1. Vào mục Hồ sơ | (không cần nhập gì) | Hiển thị đúng thông tin cá nhân và thông tin xe (biển số, loại xe, hãng xe) | High |
| TC-PROF-009 | Hồ sơ cá nhân | Tài xế cập nhật đầy đủ thông tin xe lần đầu | Chưa cập nhật xe, hồ sơ đang thiếu | 1. Vào Hồ sơ<br>2. Nhập biển số, loại xe, hãng, đời xe, upload giấy tờ<br>3. Lưu | Biển số: 59A-123.45<br>Loại xe: 4 chỗ<br>Hãng: Toyota | Hồ sơ được lưu, chuyển trạng thái "chờ duyệt" | High |
| TC-PROF-010 | Hồ sơ cá nhân | Tài xế chọn loại xe không có trong danh sách hỗ trợ | Đang cập nhật hồ sơ xe | 1. Chọn loại xe lạ (nếu can thiệp được ngoài danh sách cho phép) | Loại xe: xe 16 chỗ | App chỉ cho chọn trong danh sách được hỗ trợ (xe máy/4 chỗ/7 chỗ), không cho nhập tự do | Medium |
| TC-PROF-011 | Hồ sơ cá nhân | Tài xế cập nhật thiếu biển số xe | Đang cập nhật hồ sơ xe | 1. Chọn loại xe nhưng không nhập biển số<br>2. Lưu | Biển số: (để trống) | Lưu được thông tin đã nhập nhưng hồ sơ vẫn ở trạng thái chưa đầy đủ/chưa duyệt, tài xế chưa bật được trạng thái sẵn sàng | Medium |
| TC-PROF-012 | Hồ sơ cá nhân | Tài xế xóa tài khoản khi đang có chuyến chưa hoàn thành | Tài xế đang thực hiện 1 chuyến | 1. Vào Hồ sơ<br>2. Chọn "Xóa tài khoản" | (xác nhận xóa) | App từ chối, yêu cầu hoàn thành chuyến hiện tại trước | High |
| TC-PROF-013 | Hồ sơ cá nhân | [CẦN LÀM RÕ] Hai tài xế đăng ký trùng biển số xe | SRS chưa quy định biển số có bắt buộc duy nhất hay không | 1. Tài xế B nhập đúng biển số đã có của tài xế A | Biển số: 59A-123.45 (trùng) | Chưa có quy tắc chính thức. Thực tế nên chặn trùng biển số — cần BA xác nhận trước khi chốt kết quả mong đợi | Medium (cần làm rõ) |

## Đặt xe & tìm tài xế

| Test Case ID | Test Scenario | Test Case | Preconditions | Test Steps | Test Data | Expected Result | Priority |
|---|---|---|---|---|---|---|---|
| TC-TRIP-001 | Đặt xe & tìm tài xế | Khách hàng đặt xe thành công, có tài xế nhận ngay | Khách hàng không có chuyến đang chạy; có tài xế gần đó đang rảnh | 1. Nhập điểm đón<br>2. Nhập điểm đến<br>3. Chọn loại xe<br>4. Nhấn "Đặt xe" | Điểm đón: 123 Nguyễn Huệ<br>Điểm đến: 456 Lê Lợi<br>Loại xe: 4 chỗ | App chuyển sang màn hình "Đang tìm tài xế", sau đó báo có tài xế đã nhận, hiển thị thông tin tài xế/xe | High |
| TC-TRIP-002 | Đặt xe & tìm tài xế | Đặt xe khi đang có 1 chuyến chưa hoàn thành | Khách hàng đang có chuyến đang thực hiện | 1. Thử đặt thêm 1 chuyến mới | (bất kỳ điểm đón/đến nào) | App chặn lại, thông báo cần hoàn tất/hủy chuyến hiện tại trước khi đặt chuyến mới | High |
| TC-TRIP-003 | Đặt xe & tìm tài xế | Không nhập điểm đón | Đang ở màn hình đặt xe | 1. Bỏ trống điểm đón<br>2. Nhấn Đặt xe | Điểm đón: (để trống) | App báo lỗi, không cho gửi yêu cầu đặt xe | High |
| TC-TRIP-004 | Đặt xe & tìm tài xế | Không nhập điểm đến | Đang ở màn hình đặt xe | 1. Bỏ trống điểm đến<br>2. Nhấn Đặt xe | Điểm đến: (để trống) | App báo lỗi yêu cầu nhập điểm đến | High |
| TC-TRIP-005 | Đặt xe & tìm tài xế | Không chọn loại xe | Đang ở màn hình đặt xe | 1. Không chọn loại xe<br>2. Nhấn Đặt xe | Loại xe: (chưa chọn) | App yêu cầu chọn loại xe trước khi đặt | High |
| TC-TRIP-006 | Đặt xe & tìm tài xế | Điểm đón/đến nằm ngoài khu vực phục vụ | Đang ở màn hình đặt xe | 1. Chọn điểm đón/đến ở tỉnh/thành chưa hỗ trợ | Điểm đón: (một địa chỉ ngoài vùng phục vụ) | App thông báo khu vực này chưa được hỗ trợ, không cho đặt xe | Medium |
| TC-TRIP-007 | Đặt xe & tìm tài xế | Đặt xe nhưng không có tài xế nào rảnh gần đó | Không có tài xế nào đang sẵn sàng trong khu vực | 1. Đặt xe bình thường<br>2. Chờ hệ thống tìm tài xế | (điểm đón ở khu vực vắng tài xế) | Sau khi thử tìm trong thời gian quy định, app báo "Không tìm được tài xế phù hợp", đề xuất thử lại sau | High |
| TC-TRIP-008 | Đặt xe & tìm tài xế | Tài xế đầu tiên từ chối, hệ thống tự chuyển sang tài xế khác | Có ít nhất 2 tài xế rảnh gần điểm đón | 1. Đặt xe<br>2. Tài xế đầu tiên nhận được đề xuất nhưng từ chối | (giả lập tài xế A từ chối) | Khách hàng không thấy gián đoạn, màn hình vẫn hiển thị "Đang tìm tài xế", sau đó ghép được với tài xế khác | High |
| TC-TRIP-009 | Đặt xe & tìm tài xế | Xem vị trí tài xế theo thời gian thực sau khi đã ghép chuyến | Đã có tài xế nhận chuyến | 1. Ở màn hình theo dõi chuyến, quan sát vị trí tài xế trên bản đồ | (quan sát trong vài phút) | Vị trí tài xế trên bản đồ được cập nhật gần như liên tục, không bị đứng yên hoặc nhảy cóc bất thường | High |
| TC-TRIP-010 | Đặt xe & tìm tài xế | Xem trạng thái chuyến theo đúng thứ tự các mốc | Đã có tài xế nhận chuyến | 1. Theo dõi chuyến từ lúc nhận đến khi hoàn thành | (quan sát toàn bộ hành trình) | Trạng thái hiển thị đúng thứ tự: Tài xế đang đến → Đã đón khách → Đang di chuyển → Hoàn thành, không nhảy cóc | High |
| TC-TRIP-011 | Đặt xe & tìm tài xế | Xem lịch sử các chuyến đã đi | Đã có ít nhất vài chuyến trong quá khứ | 1. Vào mục "Lịch sử chuyến đi" | (không cần nhập gì) | Hiển thị danh sách chuyến đã thực hiện, mới nhất ở trên cùng | Medium |
| TC-TRIP-012 | Đặt xe & tìm tài xế | Lọc lịch sử chuyến theo khoảng thời gian | Có chuyến trải dài nhiều tháng | 1. Vào Lịch sử<br>2. Chọn khoảng ngày cần xem | Từ 01/01/2026 đến 31/01/2026 | Chỉ hiển thị đúng các chuyến trong khoảng thời gian đã chọn | Medium |
| TC-TRIP-013 | Đặt xe & tìm tài xế | Chọn khoảng thời gian lọc không hợp lệ (ngày kết thúc trước ngày bắt đầu) | Đang lọc lịch sử chuyến | 1. Chọn ngày bắt đầu sau ngày kết thúc | Từ 01/02/2026 đến 01/01/2026 | App báo khoảng thời gian không hợp lệ, không hiển thị kết quả sai | Low |
| TC-TRIP-014 | Đặt xe & tìm tài xế | Xem chi tiết 1 chuyến trong lịch sử | Có ít nhất 1 chuyến đã hoàn thành | 1. Vào Lịch sử<br>2. Chọn 1 chuyến bất kỳ | (chọn 1 chuyến) | Hiển thị đầy đủ thông tin: điểm đón/đến, tài xế, số tiền, thời gian | Medium |
| TC-TRIP-015 | Đặt xe & tìm tài xế | Đặt xe khi chưa đăng nhập | Chưa đăng nhập | 1. Cố mở màn hình đặt xe khi chưa đăng nhập | (chưa đăng nhập) | App chuyển hướng về màn hình đăng nhập, không cho đặt xe | High |

## Hủy & cập nhật trạng thái

| Test Case ID | Test Scenario | Test Case | Preconditions | Test Steps | Test Data | Expected Result | Priority |
|---|---|---|---|---|---|---|---|
| TC-STAT-001 | Hủy chuyến & cập nhật trạng thái | Khách hàng hủy chuyến khi đang tìm tài xế | Chuyến đang ở trạng thái tìm tài xế | 1. Vào màn hình chuyến đang chạy<br>2. Nhấn "Hủy chuyến"<br>3. Xác nhận | Lý do: "Đổi ý" | Chuyến bị hủy ngay, quay về màn hình đặt xe | High |
| TC-STAT-002 | Hủy chuyến & cập nhật trạng thái | Khách hàng hủy chuyến khi tài xế đã nhận nhưng chưa đến | Đã có tài xế nhận chuyến, chưa tới điểm đón | 1. Nhấn "Hủy chuyến"<br>2. Xác nhận | Lý do: "Chờ lâu quá" | Chuyến bị hủy; tài xế nhận được thông báo hủy và được thả về trạng thái sẵn sàng nhận chuyến khác | High |
| TC-STAT-003 | Hủy chuyến & cập nhật trạng thái | Khách hàng cố hủy chuyến khi đang trên xe di chuyển | Chuyến đang ở trạng thái đang di chuyển | 1. Vào chuyến đang chạy<br>2. Tìm nút Hủy chuyến | (không có dữ liệu nhập) | Nút hủy chuyến không còn khả dụng, hoặc nếu bấm được thì app từ chối kèm thông báo không thể hủy ở giai đoạn này | High |
| TC-STAT-004 | Hủy chuyến & cập nhật trạng thái | Khách hàng cố hủy chuyến đã hoàn thành | Chuyến đã hoàn thành | 1. Vào lịch sử, mở chuyến đã xong<br>2. Tìm chức năng hủy | (không có) | Không có tùy chọn hủy cho chuyến đã hoàn thành | Medium |
| TC-STAT-005 | Hủy chuyến & cập nhật trạng thái | Hủy chuyến mà không nhập lý do | Chuyến đang tìm tài xế | 1. Nhấn Hủy chuyến<br>2. Bỏ trống lý do<br>3. Xác nhận | Lý do: (để trống) | Vẫn hủy được bình thường (lý do là tùy chọn) | Medium |
| TC-STAT-006 | Hủy chuyến & cập nhật trạng thái | Tài xế báo "Đã đến điểm đón" | Tài xế đã được gán chuyến, đang di chuyển tới | 1. Tài xế nhấn "Đã đến nơi đón khách" | (không cần nhập gì) | Trạng thái chuyến cập nhật thành "Tài xế đã đến"; khách hàng nhận được thông báo | High |
| TC-STAT-007 | Hủy chuyến & cập nhật trạng thái | Tài xế báo "Đã đón khách" sau khi đã đến nơi | Trạng thái hiện tại là "Tài xế đã đến" | 1. Tài xế nhấn "Bắt đầu chuyến đi" / "Đã đón khách" | (không cần nhập gì) | Trạng thái chuyến chuyển sang "Đang di chuyển", bắt đầu tính thời gian/quãng đường | High |
| TC-STAT-008 | Hủy chuyến & cập nhật trạng thái | Tài xế báo "Hoàn thành chuyến" khi đã đến nơi | Chuyến đang ở trạng thái "Đang di chuyển", xe đã tới điểm đến | 1. Tài xế nhấn "Kết thúc chuyến" | (không cần nhập gì) | Chuyến chuyển sang "Hoàn thành"; hệ thống tính cước và hiển thị màn hình thanh toán cho khách hàng | High |
| TC-STAT-009 | Hủy chuyến & cập nhật trạng thái | Tài xế cố bấm "Hoàn thành" khi chưa đón khách (bỏ qua các bước) | Chuyến mới ở trạng thái "Đã được gán tài xế", chưa đến điểm đón | 1. Tài xế thử bấm thẳng nút "Kết thúc chuyến" (nếu vô tình hiện ra) | (không có) | App không cho phép/không hiển thị nút này khi chưa qua đủ các bước trước đó — không thể nhảy cóc trạng thái | High |
| TC-STAT-010 | Hủy chuyến & cập nhật trạng thái | Một tài xế khác (không được gán) cố cập nhật trạng thái của chuyến | Chuyến đang được gán cho tài xế A | 1. Tài xế B mở đúng mã chuyến của A và thử thao tác | (giả lập truy cập chéo) | App không cho phép tài xế B thao tác trên chuyến không thuộc về mình | High |
| TC-STAT-011 | Hủy chuyến & cập nhật trạng thái | Khách hàng thấy đúng trạng thái chuyến theo thời gian thực khi tài xế cập nhật | Tài xế vừa cập nhật 1 mốc trạng thái | 1. Khách hàng để màn hình theo dõi chuyến mở sẵn khi tài xế cập nhật | (quan sát) | Màn hình khách hàng tự cập nhật trạng thái mới ngay, không cần tải lại app | High |
| TC-STAT-012 | Hủy chuyến & cập nhật trạng thái | [CẦN LÀM RÕ] Hủy chuyến có bị tính phí hay không | SRS chưa chốt chính sách phí hủy | 1. Hủy chuyến khi tài xế đã đang trên đường đến gần | Lý do: "Đổi ý" | Chưa có kết quả mong đợi chính thức về phí hủy — cần BA làm rõ với khách hàng trước khi khẳng định số tiền cụ thể trong test case | Medium (cần làm rõ) |

## Đánh giá tài xế

| Test Case ID | Test Scenario | Test Case | Preconditions | Test Steps | Test Data | Expected Result | Priority |
|---|---|---|---|---|---|---|---|
| TC-RATE-001 | Đánh giá tài xế | Khách hàng đánh giá tài xế sau khi hoàn thành chuyến | Chuyến vừa hoàn thành, chưa đánh giá | 1. Ở màn hình đánh giá hiện ra sau khi hoàn thành<br>2. Chọn số sao<br>3. Viết nhận xét (tùy chọn)<br>4. Gửi | Số sao: 5<br>Nhận xét: "Tài xế rất tốt" | Gửi đánh giá thành công, điểm trung bình của tài xế được cập nhật | High |
| TC-RATE-002 | Đánh giá tài xế | Cố đánh giá khi chuyến chưa hoàn thành | Chuyến đang ở trạng thái đang di chuyển | 1. Tìm màn hình đánh giá cho chuyến này | (không có) | Không có tùy chọn đánh giá cho đến khi chuyến hoàn thành | High |
| TC-RATE-003 | Đánh giá tài xế | Đánh giá 2 lần cho cùng 1 chuyến | Chuyến đã được đánh giá 1 lần rồi | 1. Quay lại vào chuyến đó, thử đánh giá lần nữa | Số sao: 3 | App báo chuyến này đã được đánh giá rồi, không cho gửi thêm lần nữa | High |
| TC-RATE-004 | Đánh giá tài xế | Gửi đánh giá mà không chọn số sao | Đang ở màn hình đánh giá | 1. Không chọn sao<br>2. Chỉ viết nhận xét<br>3. Nhấn Gửi | Nhận xét: "Tốt" (không chọn sao) | App yêu cầu bắt buộc phải chọn số sao trước khi gửi được | High |
| TC-RATE-005 | Đánh giá tài xế | Gửi đánh giá chỉ có số sao, không viết nhận xét | Đang ở màn hình đánh giá | 1. Chọn số sao<br>2. Bỏ trống nhận xét<br>3. Gửi | Số sao: 4 (không có nhận xét) | Gửi thành công (nhận xét là tùy chọn) | Medium |
| TC-RATE-006 | Đánh giá tài xế | Xem lại đánh giá đã gửi trước đó | Đã đánh giá 1 chuyến | 1. Vào lịch sử, mở lại chuyến đã đánh giá | (xem chuyến đã đánh giá) | Hiển thị đúng số sao và nhận xét đã gửi trước đó | Medium |
| TC-RATE-007 | Đánh giá tài xế | Sửa lại đánh giá vừa gửi (trong thời gian cho phép) | Vừa đánh giá cách đây vài phút | 1. Mở lại đánh giá vừa gửi<br>2. Đổi số sao<br>3. Lưu lại | Số sao mới: 2 (đổi từ 5) | Đánh giá được cập nhật, điểm trung bình tài xế tính lại | Medium |
| TC-RATE-008 | Đánh giá tài xế | Cố sửa đánh giá đã gửi quá lâu (quá thời hạn cho phép sửa) | Đánh giá đã gửi hơn 1 ngày trước | 1. Mở lại đánh giá cũ<br>2. Thử sửa | Số sao mới: 1 | App báo đã quá thời hạn được phép chỉnh sửa đánh giá | Medium |
| TC-RATE-009 | Đánh giá tài xế | Viết nhận xét rất dài | Đang ở màn hình đánh giá | 1. Nhập đoạn nhận xét cực dài<br>2. Gửi | Nhận xét dài khoảng vài nghìn ký tự | App xử lý ổn định, có thể giới hạn độ dài hiển thị hoặc cảnh báo, không bị treo/crash | Low |
| TC-RATE-010 | Đánh giá tài xế | Đánh giá chuyến của người khác (không phải chuyến của mình) | Chuyến thuộc về khách hàng khác | 1. Cố mở màn hình đánh giá cho chuyến không phải của mình | (giả lập truy cập chéo) | App không cho phép, không hiển thị được màn hình đánh giá cho chuyến của người khác | High |

## Vận hành tài xế

| Test Case ID | Test Scenario | Test Case | Preconditions | Test Steps | Test Data | Expected Result | Priority |
|---|---|---|---|---|---|---|---|
| TC-DOPS-001 | Vận hành tài xế | Tài xế bật trạng thái sẵn sàng khi hồ sơ đã được duyệt | Hồ sơ & xe đã được duyệt (verified) | 1. Tài xế gạt công tắc "Sẵn sàng nhận chuyến" | (bật công tắc) | Trạng thái chuyển sang "Đang sẵn sàng", bắt đầu nhận được đề xuất chuyến | High |
| TC-DOPS-002 | Vận hành tài xế | Tài xế cố bật sẵn sàng khi hồ sơ/xe chưa được duyệt | Hồ sơ đang ở trạng thái chờ duyệt | 1. Tài xế gạt công tắc "Sẵn sàng nhận chuyến" | (bật công tắc) | App từ chối, thông báo cần hoàn thiện/chờ duyệt hồ sơ trước | High |
| TC-DOPS-003 | Vận hành tài xế | Tài xế tắt trạng thái sẵn sàng | Đang ở trạng thái sẵn sàng | 1. Gạt công tắc về "Ngừng nhận chuyến" | (tắt công tắc) | Trạng thái chuyển offline, không còn nhận đề xuất chuyến mới nữa | High |
| TC-DOPS-004 | Vận hành tài xế | Tài xế bị khóa tài khoản cố bật sẵn sàng | Tài khoản tài xế đang bị khóa bởi vận hành | 1. Thử gạt công tắc sẵn sàng | (bật công tắc) | App từ chối/không cho thao tác, có thể hiện thông báo tài khoản bị khóa | High |
| TC-DOPS-005 | Vận hành tài xế | Tài xế nhận được đề xuất chuyến mới và xem chi tiết | Có khách vừa đặt xe gần vị trí tài xế | 1. App hiện popup đề xuất chuyến<br>2. Xem thông tin điểm đón/đến | (quan sát nội dung đề xuất) | Hiển thị đầy đủ điểm đón, điểm đến ước lượng, khoảng cách, thời gian còn lại để phản hồi | High |
| TC-DOPS-006 | Vận hành tài xế | Tài xế chấp nhận đề xuất chuyến trong thời gian cho phép | Đề xuất chuyến đang hiện, chưa hết hạn | 1. Nhấn "Chấp nhận" | (nhấn chấp nhận) | Chuyến được gán cho tài xế này, chuyển sang màn hình chỉ đường tới điểm đón | High |
| TC-DOPS-007 | Vận hành tài xế | Tài xế từ chối đề xuất chuyến | Đề xuất chuyến đang hiện | 1. Nhấn "Từ chối" | (nhấn từ chối) | Đề xuất biến mất, tài xế vẫn ở trạng thái sẵn sàng, có thể nhận đề xuất khác | High |
| TC-DOPS-008 | Vận hành tài xế | Tài xế không phản hồi đề xuất cho đến khi hết thời gian | Đề xuất chuyến đang hiện, có đếm ngược | 1. Không thao tác gì, chờ hết thời gian đếm ngược | (không làm gì) | Đề xuất tự động biến mất, hệ thống coi như từ chối và chuyển đề xuất cho tài xế khác | High |
| TC-DOPS-009 | Vận hành tài xế | Tài xế cố chấp nhận đề xuất đã hết hạn (do mạng chậm, bấm trễ) | Đề xuất vừa hết hạn ngay lúc tài xế bấm | 1. Bấm "Chấp nhận" ngay khi đồng hồ về 0 | (nhấn chấp nhận trễ) | App báo đề xuất đã hết hạn, không nhận được chuyến này | Medium |
| TC-DOPS-010 | Vận hành tài xế | Vị trí tài xế được cập nhật liên tục trong lúc chở khách | Tài xế đang thực hiện chuyến | 1. Quan sát vị trí tài xế trên bản đồ (từ phía khách hàng) trong lúc di chuyển | (quan sát vài phút) | Vị trí cập nhật đều đặn, khách hàng thấy đúng lộ trình di chuyển thực tế | High |
| TC-DOPS-011 | Vận hành tài xế | Tài xế mất kết nối mạng tạm thời trong lúc chở khách | Tài xế đang thực hiện chuyến, tắt wifi/data tạm thời | 1. Tắt kết nối mạng vài chục giây<br>2. Bật lại | (giả lập mất mạng) | Khi có mạng trở lại, vị trí được cập nhật lại bình thường, chuyến không bị hủy/lỗi do gián đoạn ngắn | Medium |

## Thanh toán

| Test Case ID | Test Scenario | Test Case | Preconditions | Test Steps | Test Data | Expected Result | Priority |
|---|---|---|---|---|---|---|---|
| TC-PAY-001 | Thanh toán | Khách hàng thanh toán tiền mặt sau khi hoàn thành chuyến | Chuyến vừa hoàn thành, chưa thanh toán | 1. Ở màn hình thanh toán, chọn "Tiền mặt"<br>2. Đưa tiền cho tài xế<br>3. Tài xế xác nhận đã nhận tiền | Số tiền hiển thị theo cước tính được | Thanh toán được đánh dấu hoàn tất, chuyến kết thúc trọn vẹn | High |
| TC-PAY-002 | Thanh toán | Khách hàng thanh toán qua ví điện tử | Chuyến vừa hoàn thành | 1. Chọn "Thanh toán điện tử"<br>2. Xác nhận thanh toán trên ví | Phương thức: Ví điện tử X | Thanh toán xử lý qua cổng thanh toán, hiển thị kết quả thành công | High |
| TC-PAY-003 | Thanh toán | Thanh toán điện tử bị thất bại (thẻ hết hạn/không đủ tiền) | Chuyến vừa hoàn thành | 1. Chọn thanh toán điện tử<br>2. Giao dịch bị từ chối bởi ví/ngân hàng | (giả lập giao dịch lỗi) | App báo giao dịch thất bại, đề xuất thử lại hoặc chuyển sang thanh toán tiền mặt | High |
| TC-PAY-004 | Thanh toán | Khách hàng thử lại thanh toán sau khi thất bại | Giao dịch trước đó bị thất bại | 1. Nhấn "Thử lại" | (giữ nguyên phương thức hoặc đổi) | Hệ thống xử lý lại giao dịch, không tính 2 lần tiền nếu lần trước thất bại | High |
| TC-PAY-005 | Thanh toán | Chuyển từ thanh toán điện tử thất bại sang tiền mặt | Giao dịch điện tử vừa thất bại | 1. Chọn "Đổi sang tiền mặt" thay vì thử lại | (chọn tiền mặt) | Thanh toán tiền mặt được ghi nhận bình thường, không phát sinh thêm phí | Medium |
| TC-PAY-006 | Thanh toán | Kiểm tra số tiền hiển thị đúng định dạng tiền tệ Việt Nam | Chuyến đã hoàn thành, cước đã tính | 1. Xem màn hình thanh toán | (quan sát) | Số tiền hiển thị đúng định dạng VNĐ (có dấu phân cách hàng nghìn, đơn vị rõ ràng), không lẫn ký hiệu lạ | Medium |
| TC-PAY-007 | Thanh toán | [CẦN LÀM RÕ] Cước tính ra bằng 0 hoặc âm cho quãng đường rất ngắn | Công thức tính cước chưa được chốt trong SRS | 1. Đặt chuyến với quãng đường cực ngắn (dưới 100m) | (chuyến rất ngắn) | Chưa có công thức chính thức để khẳng định số tiền cụ thể. Kỳ vọng tối thiểu: số tiền hiển thị luôn dương (không phải 0 hay âm) — cần BA chốt công thức trước khi viết test case cụ thể hơn | Medium (cần làm rõ) |
| TC-PAY-008 | Thanh toán | Khách hàng không thao tác gì ở màn hình thanh toán (bỏ đó) | Chuyến đã hoàn thành, đang chờ thanh toán | 1. Không chọn phương thức nào, thoát app | (không thao tác) | Chuyến vẫn ở trạng thái chờ thanh toán khi mở lại app, không tự động mất dữ liệu | Medium |
| TC-PAY-009 | Thanh toán | Xem lại số tiền đã thanh toán trong lịch sử chuyến | Chuyến đã thanh toán xong | 1. Vào lịch sử, mở chuyến đã thanh toán | (xem lại) | Hiển thị đúng số tiền, phương thức đã dùng, không hiển thị bất kỳ thông tin thẻ/tài khoản nhạy cảm nào | High |
| TC-PAY-010 | Thanh toán | Tài xế xác nhận nhầm đã nhận tiền mặt trong khi khách chưa trả | Chuyến chờ thanh toán tiền mặt | 1. Tài xế bấm xác nhận đã nhận tiền dù khách chưa đưa | (thao tác của tài xế) | Hệ thống ghi nhận theo xác nhận của tài xế — đây là quy trình hiện tại dựa trên lòng tin giữa 2 bên, không có bước xác thực chéo từ khách hàng (ghi nhận như 1 giới hạn hiện tại của hệ thống, không phải lỗi) | Low |

## Thông báo

| Test Case ID | Test Scenario | Test Case | Preconditions | Test Steps | Test Data | Expected Result | Priority |
|---|---|---|---|---|---|---|---|
| TC-NOTI-001 | Thông báo | Khách hàng nhận thông báo khi tài xế nhận chuyến | Vừa có tài xế nhận chuyến | 1. Quan sát thông báo đẩy (push notification) trên điện thoại | (quan sát) | Nhận được thông báo kèm tên/thông tin tài xế ngay khi vừa ghép chuyến | High |
| TC-NOTI-002 | Thông báo | Khách hàng nhận thông báo khi tài xế đã đến điểm đón | Tài xế vừa bấm "Đã đến nơi" | 1. Quan sát thông báo | (quan sát) | Nhận được thông báo "Tài xế đã đến" kịp thời | High |
| TC-NOTI-003 | Thông báo | Khách hàng nhận thông báo khi không tìm được tài xế | Hệ thống không tìm được tài xế sau nhiều lần thử | 1. Quan sát thông báo/màn hình app | (quan sát) | Hiển thị rõ ràng thông báo không tìm được tài xế, gợi ý thử lại sau | High |
| TC-NOTI-004 | Thông báo | Tài xế nhận thông báo khi khách hàng hủy chuyến | Khách hàng vừa hủy chuyến đã gán cho tài xế | 1. Quan sát thông báo trên app tài xế | (quan sát) | Tài xế nhận được thông báo hủy ngay lập tức, quay lại trạng thái sẵn sàng | High |
| TC-NOTI-005 | Thông báo | Khách hàng nhận thông báo kết quả thanh toán | Vừa thực hiện thanh toán (thành công hoặc thất bại) | 1. Quan sát thông báo sau khi thanh toán | (quan sát) | Nhận được thông báo đúng với kết quả thực tế (thành công/thất bại) | High |
| TC-NOTI-006 | Thông báo | Một kênh gửi thông báo bị lỗi không làm gián đoạn chuyến đi | Giả lập dịch vụ gửi thông báo (SMS/push) đang bị lỗi | 1. Thực hiện 1 chuyến đi trọn vẹn trong lúc dịch vụ thông báo lỗi | (giả lập lỗi kênh gửi) | Chuyến đi vẫn diễn ra và hoàn thành bình thường dù thông báo có thể bị chậm/thiếu — luồng chính không bị ảnh hưởng | High |
| TC-NOTI-007 | Thông báo | Xem lại lịch sử các thông báo đã nhận | Đã nhận một số thông báo trước đó | 1. Vào mục "Thông báo" trong app | (xem danh sách) | Hiển thị danh sách thông báo đã nhận, sắp xếp theo thời gian | Medium |
| TC-NOTI-008 | Thông báo | Không nhận được thông báo trùng lặp cho cùng 1 sự kiện | Một sự kiện (vd tài xế đến) chỉ xảy ra 1 lần | 1. Theo dõi số lượng thông báo nhận được cho 1 sự kiện | (quan sát) | Chỉ nhận đúng 1 thông báo cho mỗi sự kiện, không bị gửi lặp lại nhiều lần | Medium |

## Quản trị vận hành

| Test Case ID | Test Scenario | Test Case | Preconditions | Test Steps | Test Data | Expected Result | Priority |
|---|---|---|---|---|---|---|---|
| TC-ADM-001 | Quản trị vận hành | Nhân viên vận hành tìm kiếm khách hàng theo số điện thoại | Đăng nhập bằng tài khoản vận hành | 1. Vào trang quản lý Khách hàng<br>2. Nhập SĐT vào ô tìm kiếm | SĐT: 0901234567 | Hiển thị đúng khách hàng khớp SĐT | High |
| TC-ADM-002 | Quản trị vận hành | Tài khoản khách hàng thường cố truy cập trang quản trị | Đăng nhập bằng tài khoản khách hàng thường | 1. Cố truy cập đường link/trang quản trị | (truy cập trái phép) | Hệ thống từ chối, không cho vào trang quản trị | High |
| TC-ADM-003 | Quản trị vận hành | Duyệt hồ sơ tài xế mới đăng ký (hợp lệ) | Có 1 tài xế đang chờ duyệt, hồ sơ đầy đủ, giấy tờ rõ ràng | 1. Vào danh sách tài xế chờ duyệt<br>2. Xem hồ sơ<br>3. Nhấn "Duyệt" | (duyệt hồ sơ) | Tài xế chuyển trạng thái đã duyệt, có thể bật sẵn sàng nhận chuyến sau đó | High |
| TC-ADM-004 | Quản trị vận hành | Từ chối hồ sơ tài xế không hợp lệ kèm lý do | Hồ sơ tài xế có ảnh giấy tờ mờ/thiếu | 1. Xem hồ sơ<br>2. Nhấn "Từ chối"<br>3. Nhập lý do | Lý do: "Ảnh giấy tờ không rõ" | Hồ sơ bị từ chối, tài xế nhận được lý do để bổ sung lại | High |
| TC-ADM-005 | Quản trị vận hành | Khóa tài khoản 1 tài xế vi phạm | Tài xế đang hoạt động bình thường | 1. Vào hồ sơ tài xế<br>2. Nhấn "Khóa tài khoản"<br>3. Nhập lý do | Lý do: "Vi phạm quy định vận chuyển" | Tài xế bị khóa, không đăng nhập/nhận chuyến được nữa; hành động này được ghi lại trong nhật ký hệ thống | High |
| TC-ADM-006 | Quản trị vận hành | Mở khóa lại tài khoản tài xế | Tài xế đang bị khóa | 1. Vào hồ sơ tài xế đang khóa<br>2. Nhấn "Mở khóa" | (mở khóa) | Tài xế hoạt động lại bình thường | Medium |
| TC-ADM-007 | Quản trị vận hành | Giám sát danh sách các chuyến đang diễn ra | Đang có nhiều chuyến chưa hoàn thành | 1. Vào màn hình "Chuyến đang hoạt động" | (xem danh sách) | Hiển thị đúng các chuyến chưa hoàn thành, có thể xem chi tiết từng chuyến để xử lý sự cố | High |
| TC-ADM-008 | Quản trị vận hành | Xem báo cáo doanh thu/số chuyến theo khoảng thời gian | Có dữ liệu chuyến trong tháng vừa qua | 1. Vào mục Báo cáo<br>2. Chọn khoảng thời gian<br>3. Xem kết quả | Từ 01/01/2026 đến 31/01/2026 | Hiển thị đúng tổng số chuyến, doanh thu, tỷ lệ hoàn thành/hủy, hiệu suất tài xế trong khoảng đã chọn | High |
| TC-ADM-009 | Quản trị vận hành | Xem báo cáo mà không chọn khoảng thời gian | Đang ở màn hình Báo cáo | 1. Không chọn ngày, nhấn Xem báo cáo | (để trống ngày) | App yêu cầu chọn khoảng thời gian trước khi xem báo cáo | Medium |
| TC-ADM-010 | Quản trị vận hành | Xem báo cáo cho khoảng thời gian không có dữ liệu nào | Không có chuyến nào trong khoảng ngày đã chọn | 1. Chọn 1 khoảng thời gian xa trong quá khứ, chưa vận hành | (khoảng ngày trống dữ liệu) | Hiển thị đúng số 0 cho các chỉ số, không báo lỗi hay hiển thị sai lệch | Medium |
| TC-ADM-011 | Quản trị vận hành | Quản trị viên cấp cao tạo tài khoản nhân viên vận hành mới | Đăng nhập bằng tài khoản quản trị cấp cao nhất | 1. Vào mục Quản lý nhân viên<br>2. Nhấn "Thêm tài khoản"<br>3. Nhập thông tin và chọn vai trò | Họ tên: NV A<br>SĐT: 0977000001<br>Vai trò: Nhân viên vận hành | Tài khoản mới được tạo với đúng vai trò được chọn | High |
| TC-ADM-012 | Quản trị vận hành | Nhân viên vận hành thường cố tạo tài khoản quản trị khác | Đăng nhập bằng tài khoản vận hành thường (không phải cấp cao) | 1. Cố vào chức năng "Thêm tài khoản" | (thao tác trái phép) | Hệ thống từ chối, chỉ quản trị cấp cao mới được tạo tài khoản nhân viên mới | High |
| TC-ADM-013 | Quản trị vận hành | Phân quyền lại cho 1 nhân viên vận hành | Tài khoản nhân viên đã tồn tại | 1. Vào hồ sơ nhân viên<br>2. Đổi vai trò<br>3. Lưu | Vai trò mới: Quản trị vận hành | Vai trò được cập nhật, nhân viên đó có thêm/bớt quyền tương ứng ngay ở lần đăng nhập tiếp theo | Medium |
| TC-ADM-014 | Quản trị vận hành | Xem nhật ký thao tác (audit log) của các nhân viên vận hành | Đã có một số thao tác nhạy cảm được thực hiện trước đó (khóa tài khoản, đổi quyền...) | 1. Vào mục "Nhật ký hệ thống" | (xem danh sách log) | Hiển thị đầy đủ: ai đã làm gì, vào lúc nào, với đối tượng nào — không thể chỉnh sửa hay xóa các dòng nhật ký này | High |
