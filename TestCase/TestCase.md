# Test Case — CAB System

Bộ test case được sinh trực tiếp từ `srs.md` (Bước 6 Functional Requirements, Bước 7 Business Rules, Bước 13 Acceptance Criteria) và `openapi.yaml` (41 endpoint) — không phát sinh test case ngoài phạm vi dự án CAB System.

File Excel gốc: [`CAB_Test_Cases.xlsx`](./CAB_Test_Cases.xlsx) — cùng nội dung, dùng khi cần import vào công cụ quản lý test (TestRail, Zephyr...) hoặc chỉnh sửa trực tiếp.

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

Mỗi bộ test case theo từng nghiệp vụ (mỗi sheet/mục bên dưới) được thiết kế để phủ đủ 5 tiêu chí:

1. **Positive** — test case dữ liệu đúng, luồng thành công (happy path)
2. **Negative** — dữ liệu sai/không hợp lệ, kiểm tra hệ thống từ chối đúng cách
3. **Boundary / Power** — giá trị biên, giá trị bất thường (chuỗi quá dài, SQL injection, ký tự đặc biệt...)
4. **Bỏ trống / thiếu trường** — không nhập gì, thiếu field bắt buộc
5. **Format** — sai định dạng (ngày tháng, số điện thoại, JSON, enum...)

Một số test case được đánh dấu **`[CẦN LÀM RÕ]`** — đây là các trường hợp mà `srs.md` **chưa chốt** quy tắc nghiệp vụ cụ thể (đã ghi nhận từ Bước 1, Bước 4, Bước 7 của SRS). Với các test case này, tài liệu **không tự bịa ra kết quả mong đợi**, mà ghi rõ khoảng trống cần làm rõ với BA/khách hàng trước khi chốt test case chính thức.

## Tổng quan bộ test case

| # | Sheet / Nghiệp vụ | Số test case | Endpoint liên quan |
|---|---|---|---|
| 1 | Login Test Cases | 20 | `POST /auth/login` |
| 2 | Đăng ký tài khoản | 15 | `POST /auth/register`, `POST /drivers/register` |
| 3 | Hồ sơ khách hàng | 13 | `GET/PUT/DELETE /customers/me` |
| 4 | Hồ sơ tài xế | 12 | `GET/PUT/DELETE /drivers/me` |
| 5 | Đặt xe | 20 | `POST/GET /trips`, `GET /trips/{tripId}` |
| 6 | Hủy & cập nhật trạng thái | 18 | `POST /trips/{tripId}/cancel`, `PATCH /trips/{tripId}/status` |
| 7 | Đánh giá tài xế | 14 | `POST/GET/PUT /trips/{tripId}/rating` |
| 8 | Matching (internal) | 12 | `/internal/matching/requests`, `.../driver-response` |
| 9 | Vận hành tài xế | 16 | `/drivers/me/availability`, `/trip-offers`, `/location` |
| 10 | Thanh toán | 20 | `/payments`, `/payments/{id}/retry`, `/internal/payments/webhook` |
| 11 | Thông báo | 10 | `/internal/notifications`, `/notifications` |
| 12 | Quản trị KH & Tài xế | 14 | `/admin/customers`, `/admin/drivers` |
| 13 | Quản trị Báo cáo & Audit | 14 | `/admin/reports`, `/admin/users`, `/admin/audit-logs` |
| | **Tổng cộng** | **198** | |

## Login Test Cases

| Test Case ID | Test Scenario | Test Case | Preconditions | Test Steps | Test Data | Expected Result | Priority |
|---|---|---|---|---|---|---|---|
| TC-LOGIN-001 | Người dùng đăng nhập | Đăng nhập với số điện thoại và mật khẩu hợp lệ (Khách hàng) | Tài khoản khách hàng đã đăng ký thành công qua POST /auth/register, trạng thái active | 1. Gửi POST /auth/login<br>2. Nhập phone hợp lệ<br>3. Nhập password đúng<br>4. Gửi request | phone: 0901234567<br>password: Passw0rd@123 | HTTP 201/200; trả về access_token, token_type=Bearer, expires_in, user_id; response KHÔNG chứa password | High |
| TC-LOGIN-002 | Người dùng đăng nhập | Đăng nhập với số điện thoại và mật khẩu hợp lệ (Tài xế) | Tài khoản tài xế đã đăng ký qua POST /drivers/register, chưa bị khóa (locked=false) | 1. Gửi POST /auth/login<br>2. Nhập phone hợp lệ<br>3. Nhập password đúng<br>4. Gửi request | phone: 0912345678<br>password: DriverPass@123 | HTTP 200; trả về AuthResponse hợp lệ (access_token, token_type, expires_in, user_id) cho tài khoản tài xế | High |
| TC-LOGIN-003 | Người dùng đăng nhập | Đăng nhập với số điện thoại chưa từng đăng ký | Số điện thoại không tồn tại trong hệ thống | 1. Gửi POST /auth/login<br>2. Nhập phone không tồn tại<br>3. Nhập password bất kỳ<br>4. Gửi request | phone: 0900000000<br>password: Anything@123 | HTTP 401; error_code dạng xác thực (vd AUTH_INVALID_CREDENTIALS); message chung, KHÔNG tiết lộ là sai phone hay sai password (tránh dò tài khoản) | High |
| TC-LOGIN-004 | Người dùng đăng nhập | Đăng nhập với số điện thoại đúng nhưng mật khẩu sai | Tài khoản tồn tại, đang active | 1. Gửi POST /auth/login<br>2. Nhập phone đúng<br>3. Nhập password sai<br>4. Gửi request | phone: 0901234567<br>password: WrongPass@999 | HTTP 401; không tạo access_token; message lỗi giống hệt TC-LOGIN-003 (không phân biệt sai phone/sai password) | High |
| TC-LOGIN-005 | Người dùng đăng nhập | Bỏ trống trường phone | Đang gọi API /auth/login | 1. Để trống phone<br>2. Nhập password<br>3. Gửi request | phone: (rỗng)<br>password: Passw0rd@123 | HTTP 400; error_code validation; message yêu cầu nhập phone | High |
| TC-LOGIN-006 | Người dùng đăng nhập | Bỏ trống trường password | Đang gọi API /auth/login | 1. Nhập phone<br>2. Để trống password<br>3. Gửi request | phone: 0901234567<br>password: (rỗng) | HTTP 400; error_code validation; message yêu cầu nhập password | High |
| TC-LOGIN-007 | Người dùng đăng nhập | Bỏ trống cả phone và password | Đang gọi API /auth/login | 1. Để trống phone<br>2. Để trống password<br>3. Gửi request | phone: (rỗng)<br>password: (rỗng) | HTTP 400; message liệt kê lỗi validation cho cả 2 trường | High |
| TC-LOGIN-008 | Người dùng đăng nhập | Thiếu hẳn field "password" trong JSON body (không phải rỗng, mà không có key) | API Login đang hoạt động, request gửi trực tiếp qua Postman/Swagger | 1. Gửi POST /auth/login<br>2. Body chỉ có field phone, không có key password | { "phone": "0901234567" } | HTTP 400; lỗi "required field missing" theo đúng khai báo required: [phone, password] trong openapi.yaml | High |
| TC-LOGIN-009 | Người dùng đăng nhập | Thiếu hẳn field "phone" trong JSON body | API Login đang hoạt động | 1. Gửi POST /auth/login<br>2. Body chỉ có field password, không có key phone | { "password": "Passw0rd@123" } | HTTP 400; lỗi required field missing cho phone | High |
| TC-LOGIN-010 | Người dùng đăng nhập | Số điện thoại sai định dạng (chứa chữ cái) | Đang gọi API /auth/login | 1. Nhập phone chứa ký tự chữ<br>2. Nhập password hợp lệ<br>3. Gửi request | phone: 09A1234567<br>password: Passw0rd@123 | HTTP 400; error_code định dạng phone không hợp lệ | Medium |
| TC-LOGIN-011 | Người dùng đăng nhập | Số điện thoại sai định dạng (thiếu chữ số, chỉ 4 số) | Đang gọi API /auth/login | 1. Nhập phone chỉ 4 chữ số<br>2. Nhập password hợp lệ<br>3. Gửi request | phone: 0901<br>password: Passw0rd@123 | HTTP 400; error_code định dạng phone không hợp lệ (độ dài không đủ) | Medium |
| TC-LOGIN-012 | Người dùng đăng nhập | [CẦN LÀM RÕ] Số điện thoại nhập ở định dạng quốc tế +84 cho cùng 1 số đã đăng ký dạng 0 | Tài khoản đã đăng ký bằng phone dạng 0901234567 | 1. Nhập phone dạng +84901234567 (cùng số thuê bao)<br>2. Nhập đúng password<br>3. Gửi request | phone: +84901234567<br>password: Passw0rd@123 | SRS chưa định nghĩa quy tắc chuẩn hóa định dạng phone (0xxx vs +84xxx). Kỳ vọng tối thiểu: hệ thống xử lý nhất quán (chuẩn hóa về 1 dạng khi lưu), KHÔNG được để 2 định dạng tạo thành 2 tài khoản khác nhau. Cần BA xác nhận trước khi chốt kết quả mong đợi chính xác. | Medium |
| TC-LOGIN-013 | Người dùng đăng nhập | Mật khẩu có khoảng trắng thừa ở đầu/cuối | Tài khoản tồn tại với password gốc KHÔNG có khoảng trắng | 1. Nhập phone đúng<br>2. Nhập password đúng nhưng có thêm dấu cách đầu/cuối<br>3. Gửi request | phone: 0901234567<br>password: " Passw0rd@123 " | HTTP 401 (đăng nhập thất bại) — hệ thống KHÔNG được tự ý trim() khoảng trắng của password vì sẽ làm sai lệch xác thực so với giá trị đã lưu | Medium |
| TC-LOGIN-014 | Người dùng đăng nhập | Mật khẩu có độ dài rất lớn (500 ký tự) | Đang gọi API /auth/login | 1. Nhập phone hợp lệ<br>2. Nhập password dài 500 ký tự<br>3. Gửi request | phone: 0901234567<br>password: "A" x 500 | Hệ thống xử lý ổn định: trả về 400 (nếu có giới hạn độ dài) hoặc 401 (nếu không khớp) — TUYỆT ĐỐI không được trả 500 Internal Server Error hoặc treo hệ thống | Medium |
| TC-LOGIN-015 | Người dùng đăng nhập | Kiểm tra chống SQL Injection qua trường phone/password | Đang gọi API /auth/login | 1. Nhập phone/password chứa cú pháp SQL injection<br>2. Gửi request | phone: 0901234567' OR '1'='1<br>password: ' OR '1'='1 | HTTP 400 hoặc 401; KHÔNG được bypass xác thực; KHÔNG trả về access_token; hệ thống không lộ lỗi truy vấn DB ra response | High |
| TC-LOGIN-016 | Người dùng đăng nhập | Đăng nhập bằng tài khoản tài xế đang bị khóa | Tài khoản tài xế đã bị nhân viên vận hành khóa qua PUT /admin/drivers/{driverId}/lock (locked=true, theo FR-33) | 1. Nhập đúng phone/password của tài xế đã bị khóa<br>2. Gửi request | phone: 0912345678 (driver đã bị khóa)<br>password: DriverPass@123 | HTTP 401 hoặc 403; message thông báo tài khoản đã bị khóa/vô hiệu hóa; KHÔNG cấp access_token | High |
| TC-LOGIN-017 | Người dùng đăng nhập | Gửi request với body không đúng định dạng JSON (malformed JSON) | API Login đang hoạt động | 1. Gửi POST /auth/login<br>2. Body là JSON lỗi cú pháp | { phone: 0901234567, password: }  (thiếu dấu ngoặc kép, thiếu giá trị) | HTTP 400; message báo lỗi định dạng request (malformed request body), không phải lỗi 500 | Medium |
| TC-LOGIN-018 | Người dùng đăng nhập | Response khi đăng nhập thành công không chứa thông tin nhạy cảm | Đăng nhập thành công (dùng lại dữ liệu TC-LOGIN-001) | 1. Đăng nhập thành công<br>2. Kiểm tra toàn bộ field trong response body | phone: 0901234567<br>password: Passw0rd@123 | Response CHỈ chứa access_token, token_type, expires_in, user_id đúng theo schema AuthResponse trong openapi.yaml; KHÔNG có field password hay password_hash | High |
| TC-LOGIN-019 | Người dùng đăng nhập | Access token trả về sử dụng được để gọi API khác cần xác thực | Đã đăng nhập thành công và có access_token hợp lệ | 1. Đăng nhập lấy access_token<br>2. Gọi GET /customers/me với header Authorization: Bearer <access_token> | Authorization: Bearer <access_token vừa nhận> | HTTP 200; trả về đúng hồ sơ khách hàng tương ứng với tài khoản vừa đăng nhập (đúng FR-06: xác thực & phân quyền) | High |
| TC-LOGIN-020 | Người dùng đăng nhập | [CẦN LÀM RÕ] Giới hạn số lần đăng nhập sai liên tiếp (chống brute-force) | SRS/Business Rules (srs.md) hiện CHƯA định nghĩa cơ chế khóa tài khoản sau N lần đăng nhập sai | 1. Gửi liên tiếp 10 lần POST /auth/login với password sai cho cùng 1 tài khoản<br>2. Quan sát phản hồi ở các lần cuối | phone: 0901234567<br>password: WrongPass@999 (lặp lại 10 lần) | CHƯA CÓ kết quả mong đợi chính thức — đây là khoảng trống bảo mật cần đề xuất bổ sung business rule mới (tương tự các điểm QT chưa chốt ở Bước 7 srs.md). Ghi nhận rủi ro, KHÔNG tự ý giả định con số cụ thể (vd không mặc định là khóa sau 5 lần) khi chưa có xác nhận từ BA/khách hàng | Medium (cần làm rõ) |

## Đăng ký tài khoản

| Test Case ID | Test Scenario | Test Case | Preconditions | Test Steps | Test Data | Expected Result | Priority |
|---|---|---|---|---|---|---|---|
| TC-REG-001 | Đăng ký tài khoản | Đăng ký khách hàng thành công với dữ liệu hợp lệ | API /auth/register hoạt động | 1. Gửi POST /auth/register<br>2. Nhập phone, password, full_name hợp lệ | phone: 0987000001<br>password: Passw0rd@123<br>full_name: Nguyen Van A | HTTP 201; trả về AuthResponse (access_token, user_id...) | High |
| TC-REG-002 | Đăng ký tài khoản | Đăng ký tài xế thành công với dữ liệu hợp lệ | API /drivers/register hoạt động | 1. Gửi POST /drivers/register<br>2. Nhập phone, password, full_name hợp lệ | phone: 0987000002<br>password: Passw0rd@123<br>full_name: Tran Van B | HTTP 201; trả về AuthResponse | High |
| TC-REG-003 | Đăng ký tài khoản | Đăng ký khách hàng với phone đã tồn tại | phone 0987000001 đã đăng ký ở TC-REG-001 | 1. Gửi POST /auth/register với phone trùng | phone: 0987000001<br>password: Other@123<br>full_name: Nguyen Van C | HTTP 409; error_code dạng PHONE_ALREADY_EXISTS | High |
| TC-REG-004 | Đăng ký tài khoản | Đăng ký tài xế với phone đã tồn tại | phone 0987000002 đã đăng ký ở TC-REG-002 | 1. Gửi POST /drivers/register với phone trùng | phone: 0987000002<br>password: Other@123<br>full_name: Tran Van D | HTTP 409 | High |
| TC-REG-005 | Đăng ký tài khoản | Bỏ trống phone khi đăng ký | API đang hoạt động | 1. Gửi request thiếu/rỗng phone | phone: (rỗng)<br>password: Passw0rd@123<br>full_name: Test | HTTP 400; yêu cầu nhập phone | High |
| TC-REG-006 | Đăng ký tài khoản | Bỏ trống password khi đăng ký | API đang hoạt động | 1. Gửi request thiếu/rỗng password | phone: 0987000003<br>password: (rỗng)<br>full_name: Test | HTTP 400; yêu cầu nhập password | High |
| TC-REG-007 | Đăng ký tài khoản | Bỏ trống full_name khi đăng ký | API đang hoạt động | 1. Gửi request thiếu full_name | phone: 0987000004<br>password: Passw0rd@123<br>full_name: (rỗng) | HTTP 400; yêu cầu nhập full_name (required theo openapi.yaml) | High |
| TC-REG-008 | Đăng ký tài khoản | Email sai định dạng khi đăng ký khách hàng | API /auth/register, email là field tùy chọn | 1. Nhập email không đúng định dạng<br>2. Gửi request | phone: 0987000005<br>password: Passw0rd@123<br>email: abc-khong-hop-le | HTTP 400; error_code định dạng email không hợp lệ | Medium |
| TC-REG-009 | Đăng ký tài khoản | Số điện thoại sai định dạng (chứa chữ) | API đang hoạt động | 1. Nhập phone chứa chữ cái<br>2. Gửi request | phone: 09ABC00006<br>password: Passw0rd@123 | HTTP 400 | Medium |
| TC-REG-010 | Đăng ký tài khoản | [CẦN LÀM RÕ] Mật khẩu quá ngắn (dưới 5 ký tự) | openapi.yaml hiện KHÔNG khai báo minLength cho password | 1. Nhập password ngắn<br>2. Gửi request | phone: 0987000007<br>password: 123 | Chưa có ràng buộc chính thức trong SRS/OpenAPI. Đề xuất bổ sung business rule minLength (vd 8 ký tự) — hiện tại flag để BA xác nhận, KHÔNG mặc định là hệ thống sẽ từ chối hay chấp nhận | Medium (cần làm rõ) |
| TC-REG-011 | Đăng ký tài khoản | [CẦN LÀM RÕ] Một số điện thoại vừa đăng ký Customer vừa đăng ký Driver | phone đã tồn tại ở bảng ACCOUNT với role=customer | 1. Dùng cùng phone gửi POST /drivers/register | phone: 0987000001 (đã là customer)<br>password: Passw0rd@123<br>full_name: Test Driver | SRS chưa định nghĩa 1 phone có được giữ 2 vai trò hay không. Đề xuất: từ chối (409) để tránh nhập nhằng vai trò trên cùng ACCOUNT — cần BA xác nhận trước khi chốt | Medium (cần làm rõ) |
| TC-REG-012 | Đăng ký tài khoản | Response không chứa password sau khi đăng ký | Đăng ký thành công | 1. Đăng ký thành công<br>2. Kiểm tra toàn bộ response body | phone: 0987000008<br>password: Passw0rd@123 | Response chỉ có access_token, token_type, expires_in, user_id — KHÔNG có field password | High |
| TC-REG-013 | Đăng ký tài khoản | full_name chứa dấu tiếng Việt có dấu | API đang hoạt động | 1. Nhập full_name có dấu<br>2. Gửi request | phone: 0987000009<br>full_name: Nguyễn Thị Hương | HTTP 201; lưu và trả về đúng full_name có dấu (UTF-8) | Medium |
| TC-REG-014 | Đăng ký tài khoản | full_name chứa ký tự SQL injection | API đang hoạt động | 1. Nhập full_name dạng injection<br>2. Gửi request | phone: 0987000010<br>full_name: Robert'); DROP TABLE users;-- | HTTP 201 (lưu như chuỗi ký tự thường) hoặc 400 nếu có filter; TUYỆT ĐỐI không thực thi lệnh SQL, không lộ lỗi DB | High |
| TC-REG-015 | Đăng ký tài khoản | Đăng ký khách hàng có kèm email hợp lệ | API đang hoạt động | 1. Nhập đầy đủ phone, password, full_name, email hợp lệ<br>2. Gửi request | phone: 0987000011<br>email: user@example.com | HTTP 201; đăng ký thành công, email được lưu | Medium |

## Hồ sơ khách hàng

| Test Case ID | Test Scenario | Test Case | Preconditions | Test Steps | Test Data | Expected Result | Priority |
|---|---|---|---|---|---|---|---|
| TC-CUST-001 | Hồ sơ khách hàng | Xem hồ sơ khi đã đăng nhập hợp lệ | Đã đăng nhập, có access_token | 1. Gửi GET /customers/me kèm token | Authorization: Bearer <token hợp lệ> | HTTP 200; trả về đúng full_name, avatar_url, rating_avg của chính tài khoản đó | High |
| TC-CUST-002 | Hồ sơ khách hàng | Xem hồ sơ khi không có token | Không đăng nhập | 1. Gửi GET /customers/me không kèm Authorization header | (không có token) | HTTP 401 | High |
| TC-CUST-003 | Hồ sơ khách hàng | Xem hồ sơ với token không hợp lệ/hết hạn | Token đã hết hạn hoặc sai định dạng | 1. Gửi GET /customers/me với token sai | Authorization: Bearer invalid.token.xxx | HTTP 401 | High |
| TC-CUST-004 | Hồ sơ khách hàng | Cập nhật full_name hợp lệ | Đã đăng nhập | 1. Gửi PUT /customers/me<br>2. Body có full_name mới | full_name: Nguyen Van A2 | HTTP 200; hồ sơ được cập nhật đúng full_name mới | High |
| TC-CUST-005 | Hồ sơ khách hàng | Cập nhật avatar_url hợp lệ | Đã đăng nhập | 1. Gửi PUT /customers/me với avatar_url dạng URL | avatar_url: https://cdn.cab.vn/avt/1.png | HTTP 200; avatar_url được cập nhật | Medium |
| TC-CUST-006 | Hồ sơ khách hàng | [CẦN LÀM RÕ] Cập nhật avatar_url với chuỗi không phải URL hợp lệ | Schema avatar_url chỉ khai báo type: string, KHÔNG ràng buộc format URL | 1. Gửi PUT /customers/me với avatar_url là chuỗi bất kỳ | avatar_url: "khong-phai-url" | OpenAPI hiện chấp nhận (không có validate format). Đề xuất bổ sung format: uri ở service thực tế — cần BA/dev xác nhận có validate hay không trước khi chốt kết quả mong đợi chính xác | Medium (cần làm rõ) |
| TC-CUST-007 | Hồ sơ khách hàng | Cập nhật email sai định dạng | Đã đăng nhập | 1. Gửi PUT /customers/me với email sai định dạng | email: khong-hop-le | HTTP 400 | Medium |
| TC-CUST-008 | Hồ sơ khách hàng | Cập nhật với body rỗng {} | Đã đăng nhập, không field nào là required trong PUT | 1. Gửi PUT /customers/me với body {} | {} | HTTP 200; hồ sơ giữ nguyên như trước (không có gì thay đổi), không lỗi | Medium |
| TC-CUST-009 | Hồ sơ khách hàng | Xóa tài khoản khi không có chuyến hoạt động | Không có Trip nào ở trạng thái chưa hoàn thành | 1. Gửi DELETE /customers/me | Authorization: Bearer <token> | HTTP 204; tài khoản chuyển trạng thái deleted (soft delete) | High |
| TC-CUST-010 | Hồ sơ khách hàng | Xóa tài khoản khi đang có chuyến chưa hoàn thành | Khách hàng đang có Trip status = searching_driver hoặc in_progress | 1. Gửi DELETE /customers/me | Authorization: Bearer <token> | HTTP 409; error_code dạng CUSTOMER_HAS_ACTIVE_TRIP | High |
| TC-CUST-011 | Hồ sơ khách hàng | Gọi lại DELETE trên tài khoản đã bị xóa trước đó | Tài khoản đã ở trạng thái deleted | 1. Gửi DELETE /customers/me lần 2 | Authorization: Bearer <token của tài khoản đã xóa> | HTTP 401/404 (token không còn hợp lệ do tài khoản đã bị vô hiệu hóa) | Medium |
| TC-CUST-012 | Hồ sơ khách hàng | full_name có độ dài rất lớn (1000 ký tự) | Đã đăng nhập | 1. Gửi PUT /customers/me với full_name dài 1000 ký tự | full_name: "A" x 1000 | Hệ thống xử lý ổn định: 400 (nếu có giới hạn) hoặc lưu thành công, KHÔNG trả 500 | Medium |
| TC-CUST-013 | Hồ sơ khách hàng | Đảm bảo GET /customers/me luôn trả đúng dữ liệu của chủ token (không truyền ID qua URL) | Có 2 tài khoản khách hàng A và B | 1. Đăng nhập bằng tài khoản A, lấy token A<br>2. Gọi GET /customers/me với token A | Authorization: Bearer <token A> | HTTP 200; trả về đúng hồ sơ của A, không thể xem được hồ sơ B qua endpoint này (thiết kế /me an toàn theo mặc định) | High |

## Hồ sơ tài xế

| Test Case ID | Test Scenario | Test Case | Preconditions | Test Steps | Test Data | Expected Result | Priority |
|---|---|---|---|---|---|---|---|
| TC-DRV-001 | Hồ sơ tài xế | Xem hồ sơ và phương tiện khi đã đăng nhập | Đã đăng nhập bằng tài khoản tài xế | 1. Gửi GET /drivers/me | Authorization: Bearer <token tài xế> | HTTP 200; trả về full_name, status, rating_avg và object vehicle | High |
| TC-DRV-002 | Hồ sơ tài xế | Xem hồ sơ khi không có token | Không đăng nhập | 1. Gửi GET /drivers/me không kèm token | (không có token) | HTTP 401 | High |
| TC-DRV-003 | Hồ sơ tài xế | Cập nhật đầy đủ thông tin phương tiện hợp lệ | Đã đăng nhập | 1. Gửi PUT /drivers/me<br>2. Nhập đủ plate_number, vehicle_type, brand, model, document_url | plate_number: 59A-123.45<br>vehicle_type: car_4<br>brand: Toyota<br>model: Vios<br>document_url: https://cdn.cab.vn/doc/1.pdf | HTTP 200; hồ sơ và vehicle được cập nhật đầy đủ | High |
| TC-DRV-004 | Hồ sơ tài xế | Cập nhật vehicle_type không nằm trong enum cho phép | Đã đăng nhập | 1. Gửi PUT /drivers/me với vehicle_type không hợp lệ | vehicle_type: car_16 | HTTP 400 (vi phạm enum [bike, car_4, car_7] trong openapi.yaml) | Medium |
| TC-DRV-005 | Hồ sơ tài xế | Cập nhật thiếu plate_number (hồ sơ chưa đầy đủ) | QT-07: cần đủ hồ sơ mới được bật sẵn sàng, nhưng PUT vẫn cho lưu từng phần | 1. Gửi PUT /drivers/me chỉ có vehicle_type, không có plate_number | vehicle_type: car_4 (thiếu plate_number) | HTTP 200; lưu thành công nhưng verified_status vẫn pending; việc chặn bật sẵn sàng khi thiếu hồ sơ nằm ở PATCH /drivers/me/availability, không phải ở đây | Medium |
| TC-DRV-006 | Hồ sơ tài xế | Xóa tài khoản tài xế khi không có chuyến hoạt động | Không có Trip nào chưa hoàn thành | 1. Gửi DELETE /drivers/me | Authorization: Bearer <token> | HTTP 204; tài khoản chuyển trạng thái deleted | High |
| TC-DRV-007 | Hồ sơ tài xế | Xóa tài khoản khi đang có chuyến chưa hoàn thành | Tài xế đang có Trip status = in_progress | 1. Gửi DELETE /drivers/me | Authorization: Bearer <token> | HTTP 409; error_code dạng DRIVER_HAS_ACTIVE_TRIP | High |
| TC-DRV-008 | Hồ sơ tài xế | [CẦN LÀM RÕ] document_url không phải URL hợp lệ | Schema document_url chỉ type: string, không ràng buộc format | 1. Gửi PUT /drivers/me với document_url là chuỗi bất kỳ | document_url: "abc123" | OpenAPI hiện không chặn. Cần BA/dev xác nhận có validate format tài liệu hay không trước khi chốt | Medium (cần làm rõ) |
| TC-DRV-009 | Hồ sơ tài xế | [CẦN LÀM RÕ] plate_number trùng với xe đã đăng ký bởi tài xế khác | SRS chưa định nghĩa ràng buộc unique cho biển số xe | 1. Tài xế B nhập plate_number đã tồn tại của tài xế A | plate_number: 59A-123.45 (trùng TC-DRV-003) | Chưa có business rule chính thức. Thực tế nên là unique (1 xe = 1 chủ) — cần bổ sung ràng buộc và xác nhận với BA | Medium (cần làm rõ) |
| TC-DRV-010 | Hồ sơ tài xế | Cập nhật full_name là chuỗi rỗng có gửi field | Đã đăng nhập | 1. Gửi PUT /drivers/me với full_name: "" | full_name: "" | HTTP 400 (khác với không gửi field — có gửi nhưng rỗng, nên bị coi là invalid nếu có ràng buộc minLength cho tên) | Medium |
| TC-DRV-011 | Hồ sơ tài xế | plate_number chứa ký tự SQL injection | Đã đăng nhập | 1. Gửi PUT /drivers/me với plate_number chứa injection | plate_number: 59A'; DROP TABLE vehicles;-- | HTTP 200 (lưu như chuỗi thường) hoặc 400; TUYỆT ĐỐI không thực thi SQL, không crash hệ thống | High |
| TC-DRV-012 | Hồ sơ tài xế | Kiểm tra đủ 3 giá trị hợp lệ của vehicle_type (bike/car_4/car_7) | Đã đăng nhập | 1. Lần lượt cập nhật vehicle_type = bike, car_4, car_7 | vehicle_type: bike \| car_4 \| car_7 | Cả 3 giá trị đều được chấp nhận (HTTP 200), đúng enum khai báo trong openapi.yaml | Medium |

## Đặt xe

| Test Case ID | Test Scenario | Test Case | Preconditions | Test Steps | Test Data | Expected Result | Priority |
|---|---|---|---|---|---|---|---|
| TC-TRIP-001 | Đặt xe | Đặt xe thành công với dữ liệu hợp lệ | Khách hàng đã đăng nhập, không có chuyến đang hoạt động | 1. Gửi POST /trips<br>2. Nhập pickup, dropoff, vehicle_type hợp lệ | pickup: {lat:10.77,lng:106.70,address:'A'}<br>dropoff: {lat:10.78,lng:106.72,address:'B'}<br>vehicle_type: car_4 | HTTP 201; Trip.status = searching_driver; kích hoạt Matching Service (FR-09) | High |
| TC-TRIP-002 | Đặt xe | Đặt xe khi đang có chuyến chưa hoàn thành | Khách hàng đang có 1 Trip ở trạng thái driver_assigned | 1. Gửi POST /trips lần 2 | pickup/dropoff hợp lệ khác | HTTP 409; error_code TRIP_ALREADY_ACTIVE | High |
| TC-TRIP-003 | Đặt xe | Đặt xe thiếu trường pickup | Đã đăng nhập | 1. Gửi POST /trips không có pickup | dropoff hợp lệ, vehicle_type hợp lệ, thiếu pickup | HTTP 400 (pickup là required) | High |
| TC-TRIP-004 | Đặt xe | Đặt xe thiếu trường dropoff | Đã đăng nhập | 1. Gửi POST /trips không có dropoff | pickup hợp lệ, thiếu dropoff | HTTP 400 | High |
| TC-TRIP-005 | Đặt xe | Đặt xe thiếu vehicle_type | Đã đăng nhập | 1. Gửi POST /trips không có vehicle_type | pickup, dropoff hợp lệ, thiếu vehicle_type | HTTP 400 (required theo openapi.yaml) | High |
| TC-TRIP-006 | Đặt xe | vehicle_type không nằm trong enum cho phép | Đã đăng nhập | 1. Gửi POST /trips với vehicle_type sai | vehicle_type: limousine | HTTP 400 | Medium |
| TC-TRIP-007 | Đặt xe | Điểm đón/đến ngoài vùng phục vụ | Đã đăng nhập; hệ thống có giới hạn vùng phục vụ | 1. Gửi POST /trips với tọa độ ngoài vùng phục vụ | pickup: {lat:0,lng:0,address:'Ngoai vung'} | HTTP 400; error_code PICKUP_OUT_OF_SERVICE_AREA | Medium |
| TC-TRIP-008 | Đặt xe | lat/lng không phải số (sai kiểu dữ liệu) | Đã đăng nhập | 1. Gửi POST /trips với lat là chuỗi | pickup: {lat:'abc', lng:106.7, address:'A'} | HTTP 400 (schema type: number không khớp) | Medium |
| TC-TRIP-009 | Đặt xe | address rỗng trong pickup | Đã đăng nhập | 1. Gửi POST /trips với address rỗng | pickup: {lat:10.77,lng:106.70,address:''} | HTTP 400 (address là required trong GeoPoint) | Medium |
| TC-TRIP-010 | Đặt xe | lat/lng ở giá trị biên (vd lat=90, lat=-90) | Đã đăng nhập | 1. Gửi POST /trips với lat=90 (cực Bắc) | pickup: {lat:90,lng:106.7,address:'Bien'} | Hệ thống xử lý ổn định (chấp nhận theo kiểu float hoặc từ chối do ngoài vùng phục vụ, không lỗi 500) | Low |
| TC-TRIP-011 | Đặt xe | Xem chi tiết / theo dõi chuyến đang hoạt động | Trip đã được tạo, thuộc về khách hàng đang đăng nhập | 1. Gửi GET /trips/{tripId} | tripId hợp lệ của chính khách hàng | HTTP 200; trả về đúng trạng thái hiện tại, vị trí tài xế nếu có | High |
| TC-TRIP-012 | Đặt xe | Xem chi tiết chuyến với tripId không tồn tại | Đã đăng nhập | 1. Gửi GET /trips/{tripId} với ID không tồn tại | tripId: not-exist-id | HTTP 404 | High |
| TC-TRIP-013 | Đặt xe | Xem chi tiết chuyến của người khác | Trip thuộc khách hàng A, đang đăng nhập bằng khách hàng B | 1. Gửi GET /trips/{tripId} của A bằng token B | tripId của A, token của B | HTTP 403/404 (không được xem chuyến của người khác) | High |
| TC-TRIP-014 | Đặt xe | Xem lịch sử chuyến đi (không lọc) | Khách hàng đã có nhiều chuyến trong quá khứ | 1. Gửi GET /trips | (không query param) | HTTP 200; trả về danh sách tất cả chuyến của khách hàng, sắp xếp hợp lý (mới nhất trước) | High |
| TC-TRIP-015 | Đặt xe | Xem lịch sử chuyến lọc theo status=completed | Có cả chuyến completed và cancelled | 1. Gửi GET /trips?status=completed | status: completed | HTTP 200; chỉ trả về các chuyến đã hoàn thành | Medium |
| TC-TRIP-016 | Đặt xe | Xem lịch sử chuyến lọc theo khoảng thời gian from/to | Có dữ liệu chuyến trong nhiều tháng | 1. Gửi GET /trips?from=2026-01-01&to=2026-01-31 | from: 2026-01-01<br>to: 2026-01-31 | HTTP 200; chỉ trả về chuyến trong khoảng ngày đó (định dạng ISO date) | Medium |
| TC-TRIP-017 | Đặt xe | Tham số from/to sai định dạng ngày | Đang gọi GET /trips | 1. Gửi from=31-01-2026 (sai định dạng ISO) | from: 31-01-2026 | HTTP 400; error_code định dạng ngày không hợp lệ | Medium |
| TC-TRIP-018 | Đặt xe | from lớn hơn to (khoảng thời gian không hợp lệ) | Đang gọi GET /trips | 1. Gửi from=2026-02-01&to=2026-01-01 | from: 2026-02-01<br>to: 2026-01-01 | HTTP 400; error_code khoảng thời gian không hợp lệ | Low |
| TC-TRIP-019 | Đặt xe | Phân trang danh sách lịch sử chuyến (page) | Khách hàng có hơn 20 chuyến | 1. Gửi GET /trips?page=2 | page: 2 | HTTP 200; trả về đúng trang thứ 2 theo page size mặc định | Low |
| TC-TRIP-020 | Đặt xe | Gọi POST /trips không có token | Không đăng nhập | 1. Gửi POST /trips không kèm Authorization | (không có token) | HTTP 401 | High |

## Hủy & cập nhật trạng thái

| Test Case ID | Test Scenario | Test Case | Preconditions | Test Steps | Test Data | Expected Result | Priority |
|---|---|---|---|---|---|---|---|
| TC-TSTAT-001 | Hủy & cập nhật trạng thái chuyến | Hủy chuyến khi đang ở trạng thái searching_driver | Trip.status = searching_driver | 1. Gửi POST /trips/{id}/cancel | reason: 'Đổi ý' | HTTP 200; Trip.status = cancelled, ghi cancelled_reason | High |
| TC-TSTAT-002 | Hủy & cập nhật trạng thái chuyến | Hủy chuyến khi đang ở trạng thái driver_assigned | Trip.status = driver_assigned | 1. Gửi POST /trips/{id}/cancel | reason: 'Chờ lâu quá' | HTTP 200; Trip.status = cancelled; tài xế được thông báo và quay lại trạng thái sẵn sàng | High |
| TC-TSTAT-003 | Hủy & cập nhật trạng thái chuyến | Hủy chuyến khi đã ở trạng thái in_progress (đang di chuyển) | Trip.status = in_progress | 1. Gửi POST /trips/{id}/cancel | reason: bất kỳ | HTTP 409; error_code TRIP_CANCEL_NOT_ALLOWED (theo QT-14) | High |
| TC-TSTAT-004 | Hủy & cập nhật trạng thái chuyến | Hủy chuyến đã completed | Trip.status = completed | 1. Gửi POST /trips/{id}/cancel | reason: bất kỳ | HTTP 409 | High |
| TC-TSTAT-005 | Hủy & cập nhật trạng thái chuyến | Hủy chuyến không truyền reason (reason optional) | Trip.status = searching_driver | 1. Gửi POST /trips/{id}/cancel không có body | {} (không có reason) | HTTP 200; hủy thành công, cancelled_reason có thể null/rỗng | Medium |
| TC-TSTAT-006 | Hủy & cập nhật trạng thái chuyến | Hủy chuyến của người khác | Trip thuộc khách hàng A | 1. Khách hàng B gửi POST /trips/{id_của_A}/cancel | token của B, tripId của A | HTTP 403/404 | High |
| TC-TSTAT-007 | Hủy & cập nhật trạng thái chuyến | Tài xế cập nhật trạng thái arrived_pickup khi Trip đang driver_assigned | Trip.status = driver_assigned, đúng tài xế được gán | 1. Gửi PATCH /trips/{id}/status với status=arrived_pickup | status: arrived_pickup | HTTP 200; Trip.status = arrived_pickup; khách hàng nhận thông báo | High |
| TC-TSTAT-008 | Hủy & cập nhật trạng thái chuyến | Tài xế cập nhật picked_up sau arrived_pickup | Trip.status = arrived_pickup | 1. Gửi PATCH .../status với status=picked_up | status: picked_up | HTTP 200; Trip.status = picked_up, ghi nhận bắt đầu di chuyển | High |
| TC-TSTAT-009 | Hủy & cập nhật trạng thái chuyến | Tài xế cập nhật completed sau khi đã picked_up/in_progress | Trip.status = in_progress | 1. Gửi PATCH .../status với status=completed | status: completed | HTTP 200; Trip.status = completed, ghi completed_at, tính distance_km; kích hoạt luồng thanh toán | High |
| TC-TSTAT-010 | Hủy & cập nhật trạng thái chuyến | Chuyển trạng thái SAI thứ tự: completed khi đang driver_assigned (bỏ qua các bước giữa) | Trip.status = driver_assigned (chưa arrived_pickup/picked_up) | 1. Gửi PATCH .../status với status=completed | status: completed | HTTP 409; error_code TRIP_STATUS_INVALID_TRANSITION (QT-09) | High |
| TC-TSTAT-011 | Hủy & cập nhật trạng thái chuyến | Chuyển trạng thái lùi lại (vd từ picked_up về arrived_pickup) | Trip.status = picked_up | 1. Gửi PATCH .../status với status=arrived_pickup | status: arrived_pickup | HTTP 409 (không cho lùi trạng thái, vi phạm QT-09) | Medium |
| TC-TSTAT-012 | Hủy & cập nhật trạng thái chuyến | Tài xế không thuộc chuyến cố cập nhật trạng thái | Trip được gán cho tài xế X, tài xế Y (khác) gọi API | token của tài xế Y, tripId của X | HTTP 403 (không đúng tài xế được gán cho chuyến này) | High |  |
| TC-TSTAT-013 | Hủy & cập nhật trạng thái chuyến | Gửi status không nằm trong enum hợp lệ | Trip đang driver_assigned | 1. Gửi PATCH .../status với status=flying | status: flying | HTTP 400 (vi phạm enum trong openapi.yaml) | Medium |
| TC-TSTAT-014 | Hủy & cập nhật trạng thái chuyến | Cập nhật trạng thái khi thiếu field status | Trip đang driver_assigned | 1. Gửi PATCH .../status với body {} | {} | HTTP 400 (status là required) | Medium |
| TC-TSTAT-015 | Hủy & cập nhật trạng thái chuyến | Cập nhật trạng thái cho chuyến đã cancelled | Trip.status = cancelled | 1. Gửi PATCH .../status với status=picked_up | status: picked_up | HTTP 409 (chuyến đã kết thúc vòng đời, không cho cập nhật tiếp) | Medium |
| TC-TSTAT-016 | Hủy & cập nhật trạng thái chuyến | Khách hàng (không phải tài xế) gọi API PATCH .../status | Đã đăng nhập bằng tài khoản khách hàng | 1. Khách hàng gửi PATCH /trips/{id}/status | token của khách hàng | HTTP 403 (chỉ tài xế được phép cập nhật trạng thái theo thiết kế) | High |
| TC-TSTAT-017 | Hủy & cập nhật trạng thái chuyến | Gọi hủy chuyến với tripId không tồn tại | Đã đăng nhập | 1. Gửi POST /trips/not-exist-id/cancel | tripId: not-exist-id | HTTP 404 | Medium |
| TC-TSTAT-018 | Hủy & cập nhật trạng thái chuyến | [CẦN LÀM RÕ] Hủy chuyến có tính phí hủy hay không | QT-14 ghi nhận chính sách phí hủy CHƯA được khách hàng chốt | 1. Hủy chuyến ở trạng thái driver_assigned (tài xế đã di chuyển đến gần) | reason: 'Đổi ý' | Chưa có kết quả mong đợi chính thức về phí hủy — cần BA làm rõ với khách hàng trước khi viết test case khẳng định số tiền cụ thể | Medium (cần làm rõ) |

## Đánh giá tài xế

| Test Case ID | Test Scenario | Test Case | Preconditions | Test Steps | Test Data | Expected Result | Priority |
|---|---|---|---|---|---|---|---|
| TC-RATE-001 | Đánh giá tài xế | Đánh giá thành công với số sao hợp lệ (1-5) | Trip.status = completed, chưa được đánh giá | 1. Gửi POST /trips/{id}/rating<br>2. Nhập stars hợp lệ | stars: 5<br>comment: 'Tài xế rất tốt' | HTTP 201; Rating được tạo; rating_avg của tài xế được cập nhật lại (QT-17) | High |
| TC-RATE-002 | Đánh giá tài xế | Đánh giá khi chuyến chưa hoàn thành | Trip.status = in_progress | 1. Gửi POST /trips/{id}/rating | stars: 5 | HTTP 409; error_code TRIP_NOT_COMPLETED | High |
| TC-RATE-003 | Đánh giá tài xế | Đánh giá 2 lần cho cùng 1 chuyến | Trip đã được đánh giá 1 lần (TC-RATE-001) | 1. Gửi POST /trips/{id}/rating lần thứ 2 | stars: 3 | HTTP 409; error_code TRIP_ALREADY_RATED (QT-18) | High |
| TC-RATE-004 | Đánh giá tài xế | stars ngoài khoảng cho phép (vd 0 hoặc 6) | Trip.status = completed, chưa đánh giá | 1. Gửi POST /trips/{id}/rating với stars=0 | stars: 0 | HTTP 400 (vi phạm minimum:1, maximum:5 trong openapi.yaml) | Medium |
| TC-RATE-005 | Đánh giá tài xế | stars = 6 (vượt maximum) | Trip.status = completed, chưa đánh giá | 1. Gửi POST /trips/{id}/rating với stars=6 | stars: 6 | HTTP 400 | Medium |
| TC-RATE-006 | Đánh giá tài xế | Bỏ trống stars (chỉ có comment) | Trip.status = completed | 1. Gửi POST /trips/{id}/rating không có stars | comment: 'Tốt' (thiếu stars) | HTTP 400 (stars là required) | High |
| TC-RATE-007 | Đánh giá tài xế | stars không phải số nguyên (vd 4.5) | Trip.status = completed | 1. Gửi POST /trips/{id}/rating với stars=4.5 | stars: 4.5 | HTTP 400 (schema type: integer) | Low |
| TC-RATE-008 | Đánh giá tài xế | Đánh giá không kèm comment (comment optional) | Trip.status = completed, chưa đánh giá | 1. Gửi POST /trips/{id}/rating chỉ có stars | stars: 4 (không có comment) | HTTP 201; tạo thành công, comment rỗng/null | Medium |
| TC-RATE-009 | Đánh giá tài xế | Xem lại đánh giá đã gửi cho chuyến | Trip đã được đánh giá | 1. Gửi GET /trips/{id}/rating | tripId đã có rating | HTTP 200; trả về đúng stars, comment đã gửi | Medium |
| TC-RATE-010 | Đánh giá tài xế | Xem đánh giá cho chuyến chưa được đánh giá | Trip.status = completed nhưng chưa rating | 1. Gửi GET /trips/{id}/rating | tripId chưa có rating | HTTP 404 | Medium |
| TC-RATE-011 | Đánh giá tài xế | Sửa đánh giá trong vòng 24 giờ (cho phép) | Rating vừa tạo cách đây 1 giờ | 1. Gửi PUT /trips/{id}/rating với stars mới | stars: 2 (sửa lại từ 5) | HTTP 200; Rating được cập nhật, rating_avg tài xế tính lại | Medium |
| TC-RATE-012 | Đánh giá tài xế | Sửa đánh giá sau khi đã quá 24 giờ | Rating được tạo cách đây hơn 24 giờ | 1. Gửi PUT /trips/{id}/rating | stars: 1 | HTTP 409; error_code RATING_EDIT_WINDOW_EXPIRED | Medium |
| TC-RATE-013 | Đánh giá tài xế | comment chứa nội dung rất dài (vd 5000 ký tự) | Trip.status = completed | 1. Gửi POST .../rating với comment 5000 ký tự | comment: "A" x 5000 | Hệ thống xử lý ổn định: chấp nhận hoặc trả 400 nếu có giới hạn, không lỗi 500 | Low |
| TC-RATE-014 | Đánh giá tài xế | Đánh giá chuyến của người khác | Trip thuộc khách hàng A | 1. Khách hàng B gửi POST /trips/{id_của_A}/rating | token B, tripId của A | HTTP 403/404 | High |

## Matching (internal)

| Test Case ID | Test Scenario | Test Case | Preconditions | Test Steps | Test Data | Expected Result | Priority |
|---|---|---|---|---|---|---|---|
| TC-MATCH-001 | Matching (internal) | Tìm và ghép được tài xế ngay lần đề xuất đầu tiên | Có ít nhất 1 tài xế available gần pickup, Trip vừa được tạo | 1. Trip Service gọi POST /internal/matching/requests | trip_id, pickup, vehicle_type hợp lệ | HTTP 202; MatchingRequest.status chuyển matched sau khi tài xế chấp nhận; Trip.status = driver_assigned (QT-01,02) | High |
| TC-MATCH-002 | Matching (internal) | Fallback khi tài xế đầu tiên từ chối | Đã gửi đề xuất cho tài xế A, còn tài xế B phù hợp | 1. Gửi POST .../driver-response với response=rejected cho tài xế A | driver_id: A, response: rejected | HTTP 200; hệ thống tự động chuyển đề xuất sang tài xế B (QT-04) | High |
| TC-MATCH-003 | Matching (internal) | Fallback khi tài xế không phản hồi (timeout) | Đã gửi đề xuất cho tài xế A, quá thời gian phản hồi | 1. Gửi POST .../driver-response với response=timeout | driver_id: A, response: timeout | HTTP 200; xử lý như bị từ chối, chuyển sang tài xế tiếp theo | High |
| TC-MATCH-004 | Matching (internal) | Không tìm được tài xế sau khi hết danh sách | Toàn bộ tài xế phù hợp đều đã từ chối/timeout | 1. Gửi driver-response reject cho tài xế cuối cùng trong danh sách | driver_id cuối cùng, response: rejected | MatchingRequest.status = no_driver_found; Trip.status = no_driver_found; khách hàng nhận thông báo (QT-05) | High |
| TC-MATCH-005 | Matching (internal) | Không có tài xế nào available trong khu vực (ngay từ đầu) | Không có tài xế available gần pickup | 1. Gửi POST /internal/matching/requests | pickup ở khu vực không có tài xế | HTTP 202 nhưng nhanh chóng chuyển no_driver_found | High |
| TC-MATCH-006 | Matching (internal) | Truy vấn trạng thái 1 matching request đang xử lý | MatchingRequest đang ở trạng thái offering | 1. Gửi GET /internal/matching/requests/{id} | requestId hợp lệ | HTTP 200; trả về status hiện tại và current_offer_driver_id | Medium |
| TC-MATCH-007 | Matching (internal) | Truy vấn matching request không tồn tại | API đang hoạt động | 1. Gửi GET /internal/matching/requests/not-exist | requestId: not-exist | HTTP 404 | Medium |
| TC-MATCH-008 | Matching (internal) | Ghi nhận phản hồi cho request đã kết thúc (đã matched) | MatchingRequest.status đã là matched | 1. Gửi POST .../driver-response cho request đã matched | driver_id khác, response: accepted | HTTP 409 (request đã kết thúc, không xử lý tiếp) | Medium |
| TC-MATCH-009 | Matching (internal) | Gọi API internal thiếu API Key | API đang hoạt động, yêu cầu X-Internal-Api-Key | 1. Gửi POST /internal/matching/requests không có header key | (không có X-Internal-Api-Key) | HTTP 401 (không xác thực được service gọi) | High |
| TC-MATCH-010 | Matching (internal) | Gọi API internal với API Key sai | API đang hoạt động | 1. Gửi request với X-Internal-Api-Key sai | X-Internal-Api-Key: wrong-key | HTTP 401 | High |
| TC-MATCH-011 | Matching (internal) | Trip Service gửi trip_id không tồn tại để yêu cầu matching | API đang hoạt động | 1. Gửi POST /internal/matching/requests với trip_id giả | trip_id: not-exist-trip | HTTP 400 (dữ liệu không hợp lệ) | Medium |
| TC-MATCH-012 | Matching (internal) | [CẦN LÀM RÕ] Một tài xế chỉ được nhận tối đa 1 chuyến tại 1 thời điểm | QT-06 là suy luận hợp lý, cần xác nhận lại chính thức | 1. Gửi đề xuất chuyến B cho tài xế đang thực hiện chuyến A (chưa hoàn thành) | driver_id đang bận với 1 Trip khác | Kỳ vọng: tài xế này KHÔNG được đưa vào danh sách matching cho chuyến B — cần BA xác nhận đây là rule bắt buộc trước khi chốt test | Medium (cần làm rõ) |

## Vận hành tài xế

| Test Case ID | Test Scenario | Test Case | Preconditions | Test Steps | Test Data | Expected Result | Priority |
|---|---|---|---|---|---|---|---|
| TC-DOPS-001 | Vận hành tài xế | Bật trạng thái sẵn sàng khi hồ sơ & phương tiện đã đầy đủ và verified | Driver.verified_status = verified, đủ thông tin xe | 1. Gửi PATCH /drivers/me/availability | status: available | HTTP 200; Driver.status = available | High |
| TC-DOPS-002 | Vận hành tài xế | Bật trạng thái sẵn sàng khi hồ sơ/phương tiện CHƯA đầy đủ | Driver.verified_status = pending (thiếu giấy tờ) | 1. Gửi PATCH /drivers/me/availability | status: available | HTTP 400; error_code DRIVER_PROFILE_INCOMPLETE (QT-07) | High |
| TC-DOPS-003 | Vận hành tài xế | Tắt trạng thái sẵn sàng (chuyển về offline) | Driver.status = available | 1. Gửi PATCH /drivers/me/availability | status: offline | HTTP 200; Driver.status = offline; không còn được đưa vào danh sách matching | High |
| TC-DOPS-004 | Vận hành tài xế | Gửi giá trị status không hợp lệ (ngoài enum) | Đã đăng nhập | 1. Gửi PATCH /drivers/me/availability | status: busy_working | HTTP 400 (chỉ chấp nhận available/offline theo openapi.yaml) | Medium |
| TC-DOPS-005 | Vận hành tài xế | Bật sẵn sàng khi tài khoản đang bị khóa | Driver.locked = true (bị admin khóa) | 1. Gửi PATCH /drivers/me/availability | status: available | HTTP 401/403 (tài khoản bị khóa không được thao tác) | High |
| TC-DOPS-006 | Vận hành tài xế | Xem thông tin đề xuất chuyến hợp lệ | Có 1 TripOffer đang pending dành cho tài xế này | 1. Gửi GET /drivers/me/trip-offers/{offerId} | offerId hợp lệ | HTTP 200; trả về pickup_address, dropoff_address, estimated_distance_km, expires_at | High |
| TC-DOPS-007 | Vận hành tài xế | Xem đề xuất chuyến đã hết hạn hoặc không tồn tại | offerId không tồn tại hoặc đã expired | 1. Gửi GET /drivers/me/trip-offers/{offerId} | offerId: not-exist hoặc đã expired | HTTP 404 | Medium |
| TC-DOPS-008 | Vận hành tài xế | Chấp nhận đề xuất chuyến trong thời gian cho phép | TripOffer.status = pending, chưa hết hạn | 1. Gửi POST .../trip-offers/{id}/respond | decision: accept | HTTP 200; TripOffer.status = accepted; Matching Service được gọi cập nhật (FR-16) | High |
| TC-DOPS-009 | Vận hành tài xế | Từ chối đề xuất chuyến | TripOffer.status = pending | 1. Gửi POST .../trip-offers/{id}/respond | decision: reject | HTTP 200; TripOffer.status = rejected; tài xế vẫn ở trạng thái available | High |
| TC-DOPS-010 | Vận hành tài xế | Phản hồi đề xuất đã hết hạn (quá thời gian quy định) | TripOffer.status đã chuyển expired | 1. Gửi POST .../trip-offers/{id}/respond | decision: accept (nhưng đã hết hạn) | HTTP 409; error_code OFFER_EXPIRED | High |
| TC-DOPS-011 | Vận hành tài xế | Phản hồi đề xuất đã được xử lý trước đó (double respond) | TripOffer.status đã là accepted | 1. Gửi POST .../trip-offers/{id}/respond lần 2 | decision: reject | HTTP 409 | Medium |
| TC-DOPS-012 | Vận hành tài xế | Gửi decision không hợp lệ (ngoài accept/reject) | TripOffer.status = pending | 1. Gửi POST .../trip-offers/{id}/respond | decision: maybe | HTTP 400 | Medium |
| TC-DOPS-013 | Vận hành tài xế | Gửi vị trí hợp lệ trong lúc thực hiện chuyến | Tài xế đang thực hiện Trip in_progress | 1. Gửi POST /drivers/me/location | lat: 10.775<br>lng: 106.700<br>trip_id: <id chuyến hiện tại> | HTTP 204; vị trí được ghi vào Driver Location Log, khách hàng thấy cập nhật real-time | High |
| TC-DOPS-014 | Vận hành tài xế | Gửi vị trí không kèm trip_id (khi tài xế rảnh, không có chuyến) | Tài xế đang available, không có Trip | 1. Gửi POST /drivers/me/location không có trip_id | lat: 10.775<br>lng: 106.700 (không có trip_id) | HTTP 204; cập nhật current_lat/current_lng của Driver, không gắn với chuyến nào | Medium |
| TC-DOPS-015 | Vận hành tài xế | Gửi tọa độ không hợp lệ (lat/lng ngoài phạm vi) | Đã đăng nhập | 1. Gửi POST /drivers/me/location với lat=999 | lat: 999<br>lng: 106.700 | HTTP 400; error_code tọa độ không hợp lệ | Medium |
| TC-DOPS-016 | Vận hành tài xế | Thiếu trường lat hoặc lng khi gửi vị trí | Đã đăng nhập | 1. Gửi POST /drivers/me/location chỉ có lat | lat: 10.775 (thiếu lng) | HTTP 400 (lat, lng đều required) | Medium |

## Thanh toán

| Test Case ID | Test Scenario | Test Case | Preconditions | Test Steps | Test Data | Expected Result | Priority |
|---|---|---|---|---|---|---|---|
| TC-PAY-001 | Thanh toán | Tạo thanh toán tiền mặt cho chuyến đã hoàn thành | Trip.status = completed, chưa có Payment thành công | 1. Gửi POST /payments<br>2. method=cash | trip_id: <id><br>method: cash | HTTP 201; Payment.status chờ tài xế xác nhận, sau đó = success (FR-23,24) | High |
| TC-PAY-002 | Thanh toán | Tạo thanh toán điện tử cho chuyến đã hoàn thành | Trip.status = completed | 1. Gửi POST /payments<br>2. method=e_wallet | trip_id: <id><br>method: e_wallet | HTTP 201; hệ thống gọi cổng thanh toán bên thứ 3; Payment.status = processing | High |
| TC-PAY-003 | Thanh toán | Tạo thanh toán khi Trip chưa hoàn thành | Trip.status = in_progress | 1. Gửi POST /payments | trip_id: <trip đang in_progress><br>method: cash | HTTP 400; error_code TRIP_NOT_COMPLETED | High |
| TC-PAY-004 | Thanh toán | Tạo thanh toán khi đã có Payment thành công trước đó | Trip đã có Payment.status=success | 1. Gửi POST /payments lần 2 cho cùng trip_id | trip_id: <đã thanh toán> | HTTP 400; error_code PAYMENT_ALREADY_COMPLETED | High |
| TC-PAY-005 | Thanh toán | method không hợp lệ (ngoài cash/e_wallet) | Trip.status = completed | 1. Gửi POST /payments với method sai | method: bitcoin | HTTP 400 (vi phạm enum) | Medium |
| TC-PAY-006 | Thanh toán | Thiếu trường trip_id | Đã đăng nhập | 1. Gửi POST /payments không có trip_id | method: cash (thiếu trip_id) | HTTP 400 (trip_id là required) | High |
| TC-PAY-007 | Thanh toán | Xem trạng thái giao dịch hợp lệ | Payment đã tồn tại | 1. Gửi GET /payments/{paymentId} | paymentId hợp lệ | HTTP 200; trả về đúng method, status, amount | High |
| TC-PAY-008 | Thanh toán | Xem giao dịch không tồn tại | Đã đăng nhập | 1. Gửi GET /payments/not-exist-id | paymentId: not-exist-id | HTTP 404 | Medium |
| TC-PAY-009 | Thanh toán | Xem giao dịch của người khác | Payment thuộc khách hàng A | 1. Khách hàng B gọi GET /payments/{id_của_A} | token B, paymentId của A | HTTP 403/404 | High |
| TC-PAY-010 | Thanh toán | Thử lại thanh toán khi giao dịch trước đó failed | Payment.status = failed | 1. Gửi POST /payments/{id}/retry | (giữ nguyên method cũ) | HTTP 200; tạo lượt xử lý mới, Payment.status chuyển processing/pending (QT-12) | High |
| TC-PAY-011 | Thanh toán | Thử lại nhưng đổi sang phương thức khác (từ e_wallet sang cash) | Payment.status = failed, method cũ = e_wallet | 1. Gửi POST /payments/{id}/retry với method=cash | method: cash | HTTP 200; retry với phương thức mới | Medium |
| TC-PAY-012 | Thanh toán | Thử lại khi giao dịch CHƯA ở trạng thái failed (vd đang processing) | Payment.status = processing | 1. Gửi POST /payments/{id}/retry | (không có body) | HTTP 409; error_code PAYMENT_RETRY_NOT_ALLOWED | High |
| TC-PAY-013 | Thanh toán | Thử lại giao dịch đã success | Payment.status = success | 1. Gửi POST /payments/{id}/retry | (không có body) | HTTP 409 | Medium |
| TC-PAY-014 | Thanh toán | Webhook nhận kết quả thành công từ cổng thanh toán | Payment.status = processing, chữ ký hợp lệ | 1. Cổng thanh toán gọi POST /internal/payments/webhook | status: success<br>gateway_transaction_id: gw-001<br>X-Gateway-Signature hợp lệ | HTTP 200; Payment.status chuyển success; khách hàng nhận thông báo (FR-25) | High |
| TC-PAY-015 | Thanh toán | Webhook nhận kết quả thất bại từ cổng thanh toán | Payment.status = processing | 1. Cổng thanh toán gọi webhook với status=failed | status: failed | HTTP 200 (đã nhận webhook); Payment.status chuyển failed; khách hàng được đề xuất thử lại/chuyển tiền mặt (QT-12) | High |
| TC-PAY-016 | Thanh toán | Webhook với chữ ký không hợp lệ | Request giả mạo, sai X-Gateway-Signature | 1. Gửi POST /internal/payments/webhook với chữ ký sai | X-Gateway-Signature: fake-signature | HTTP 401; error_code WEBHOOK_SIGNATURE_INVALID; KHÔNG cập nhật Payment.status | High |
| TC-PAY-017 | Thanh toán | Webhook gọi trùng lặp 2 lần cho cùng 1 giao dịch (idempotency) | Webhook thành công đã xử lý 1 lần | 1. Gửi lại đúng webhook đó lần thứ 2 | gateway_transaction_id trùng với lần trước | Hệ thống không xử lý trùng (idempotent); không cộng dồn/ghi đè sai lệch dữ liệu | Medium |
| TC-PAY-018 | Thanh toán | Kiểm tra response Payment KHÔNG chứa thông tin thẻ/tài khoản nhạy cảm | Payment bất kỳ | 1. Gửi GET /payments/{id}<br>2. Kiểm tra toàn bộ field | paymentId hợp lệ | Response chỉ có id, trip_id, method, status, amount, gateway_transaction_id, paid_at — KHÔNG có số thẻ/tài khoản (đúng FR-26) | High |
| TC-PAY-019 | Thanh toán | amount tính đúng theo đơn vị tiền tệ (định dạng số) | Trip đã hoàn thành với distance_km xác định | 1. Tạo Payment cho Trip đó<br>2. Kiểm tra amount trả về | trip_id hợp lệ | amount là số dương, đúng định dạng số thực (float), không có ký tự tiền tệ lẫn trong giá trị số (đơn vị nêu riêng ở tài liệu, không lẫn vào field number) | Medium |
| TC-PAY-020 | Thanh toán | [CẦN LÀM RÕ] Công thức tính cước cho ra amount = 0 hoặc âm | QT-10: công thức tính cước CHƯA được khách hàng chốt | 1. Tạo Trip với quãng đường rất ngắn (gần 0km)<br>2. Tạo Payment | distance_km: 0.05 | Chưa có công thức chính thức nên chưa xác định được amount tối thiểu (giá mở cửa). Cần BA chốt công thức trước khi viết test case khẳng định con số cụ thể — hiện chỉ kỳ vọng amount > 0 (không được ra số âm hoặc 0) | Medium (cần làm rõ) |

## Thông báo

| Test Case ID | Test Scenario | Test Case | Preconditions | Test Steps | Test Data | Expected Result | Priority |
|---|---|---|---|---|---|---|---|
| TC-NOTI-001 | Thông báo | Gửi thông báo hợp lệ cho khách hàng | Có sự kiện cần thông báo (vd tài xế nhận chuyến) | 1. Trip Service gọi POST /internal/notifications | recipient_type: customer<br>recipient_id: <id><br>channel: push<br>content: 'Tài xế đã nhận chuyến' | HTTP 202; Notification được tạo với status=queued, sau đó chuyển sent | High |
| TC-NOTI-002 | Thông báo | Gửi thông báo hợp lệ cho tài xế | Có sự kiện cần thông báo (vd khách hủy chuyến) | 1. Gửi POST /internal/notifications | recipient_type: driver<br>recipient_id: <id><br>content: 'Chuyến đã bị hủy' | HTTP 202 | High |
| TC-NOTI-003 | Thông báo | Thiếu trường content | API đang hoạt động | 1. Gửi POST /internal/notifications không có content | recipient_type: customer, recipient_id, channel (thiếu content) | HTTP 400 (content là required) | Medium |
| TC-NOTI-004 | Thông báo | recipient_type không hợp lệ (ngoài customer/driver) | API đang hoạt động | 1. Gửi POST /internal/notifications với recipient_type sai | recipient_type: admin | HTTP 400 (vi phạm enum) | Medium |
| TC-NOTI-005 | Thông báo | Gửi thông báo với channel mới lạ (kiểm tra tính mở rộng - FR-31) | API đang hoạt động, channel là string tự do | 1. Gửi POST /internal/notifications với channel chưa dùng trước đây | channel: zalo_oa | HTTP 202; hệ thống chấp nhận channel mới mà không cần sửa schema (đúng thiết kế adapter FR-31) | Medium |
| TC-NOTI-006 | Thông báo | Gọi API internal thiếu X-Internal-Api-Key | API đang hoạt động | 1. Gửi POST /internal/notifications không có API key | (không có X-Internal-Api-Key) | HTTP 401 | High |
| TC-NOTI-007 | Thông báo | Kênh gửi thất bại (vd SMS provider down) không làm crash luồng chính | Notification Service giả lập lỗi kênh gửi | 1. Gửi thông báo trong lúc kênh push bị lỗi | channel: push (giả lập lỗi) | Notification.status = failed; NHƯNG luồng nghiệp vụ chính (Trip/Payment) vẫn tiếp tục bình thường, không bị gián đoạn (QT-20) | High |
| TC-NOTI-008 | Thông báo | Xem lịch sử thông báo của bản thân | Người dùng đã nhận một số thông báo trước đó | 1. Gửi GET /notifications | Authorization: Bearer <token> | HTTP 200; trả về danh sách thông báo của đúng người dùng đó | Medium |
| TC-NOTI-009 | Thông báo | Xem lịch sử thông báo khi không có token | Không đăng nhập | 1. Gửi GET /notifications không kèm token | (không có token) | HTTP 401 | Medium |
| TC-NOTI-010 | Thông báo | Phân trang danh sách thông báo | Người dùng có hơn 20 thông báo | 1. Gửi GET /notifications?page=2 | page: 2 | HTTP 200; trả về đúng trang 2 | Low |

## Quản trị KH & Tài xế

| Test Case ID | Test Scenario | Test Case | Preconditions | Test Steps | Test Data | Expected Result | Priority |
|---|---|---|---|---|---|---|---|
| TC-ADMU-001 | Quản trị khách hàng & tài xế | Nhân viên vận hành tìm kiếm khách hàng theo số điện thoại | Đăng nhập bằng tài khoản operator | 1. Gửi GET /admin/customers?search=0901234567 | search: 0901234567 | HTTP 200; trả về đúng khách hàng khớp số điện thoại | High |
| TC-ADMU-002 | Quản trị khách hàng & tài xế | Khách hàng thường (không phải operator) gọi API admin | Đăng nhập bằng tài khoản khách hàng thường | 1. Gửi GET /admin/customers | token của khách hàng | HTTP 403 (QT-21: sai vai trò) | High |
| TC-ADMU-003 | Quản trị khách hàng & tài xế | Chỉnh sửa thông tin khách hàng hợp lệ | customerId tồn tại | 1. Gửi PUT /admin/customers/{id} | full_name: 'Đã cập nhật' | HTTP 200; thông tin được cập nhật | Medium |
| TC-ADMU-004 | Quản trị khách hàng & tài xế | Chỉnh sửa khách hàng với customerId không tồn tại | Đăng nhập operator | 1. Gửi PUT /admin/customers/not-exist | full_name: 'X' | HTTP 404 | Medium |
| TC-ADMU-005 | Quản trị khách hàng & tài xế | Liệt kê tài xế lọc theo verified_status=pending | Có tài xế ở nhiều trạng thái verified khác nhau | 1. Gửi GET /admin/drivers?verified_status=pending | verified_status: pending | HTTP 200; chỉ trả về tài xế đang chờ duyệt | High |
| TC-ADMU-006 | Quản trị khách hàng & tài xế | Duyệt hồ sơ tài xế thành công (verified) | Driver.verified_status = pending, hồ sơ đầy đủ | 1. Gửi PUT /admin/drivers/{id}/verify | verified_status: verified | HTTP 200; tài xế có thể bật trạng thái sẵn sàng sau đó (mở khóa QT-07) | High |
| TC-ADMU-007 | Quản trị khách hàng & tài xế | Từ chối hồ sơ tài xế (rejected) kèm ghi chú lý do | Driver.verified_status = pending | 1. Gửi PUT /admin/drivers/{id}/verify | verified_status: rejected<br>note: 'Ảnh giấy tờ mờ' | HTTP 200; tài xế vẫn không thể bật sẵn sàng, note được lưu lại | High |
| TC-ADMU-008 | Quản trị khách hàng & tài xế | Khóa tài khoản tài xế kèm lý do | Driver.locked = false | 1. Gửi PUT /admin/drivers/{id}/lock | locked: true<br>reason: 'Vi phạm quy định' | HTTP 200; Driver.locked = true; hành động được ghi audit log (QT-22, FR-37) | High |
| TC-ADMU-009 | Quản trị khách hàng & tài xế | Mở khóa tài khoản tài xế | Driver.locked = true | 1. Gửi PUT /admin/drivers/{id}/lock | locked: false | HTTP 200; Driver.locked = false | Medium |
| TC-ADMU-010 | Quản trị khách hàng & tài xế | Giám sát danh sách chuyến đang diễn ra | Có nhiều Trip ở trạng thái khác completed/cancelled | 1. Gửi GET /admin/trips/active | (operator token) | HTTP 200; chỉ trả về Trip chưa kết thúc (searching_driver, driver_assigned, in_progress...) | High |
| TC-ADMU-011 | Quản trị khách hàng & tài xế | Nhân viên vận hành cấp thấp (support_staff) cố khóa tài khoản tài xế | Tài khoản đăng nhập có role=support_staff (không phải operator/super_admin) | 1. Gửi PUT /admin/drivers/{id}/lock | locked: true | HTTP 403 (không đủ quyền theo QT-21, tùy phân quyền chi tiết cần BA xác nhận role nào được lock) | Medium |
| TC-ADMU-012 | Quản trị khách hàng & tài xế | Tìm kiếm khách hàng không có kết quả khớp | search không khớp bất kỳ khách hàng nào | 1. Gửi GET /admin/customers?search=xyz-not-found | search: xyz-not-found | HTTP 200; trả về mảng rỗng [] | Low |
| TC-ADMU-013 | Quản trị khách hàng & tài xế | verified_status gửi giá trị ngoài enum khi duyệt hồ sơ | Đăng nhập operator | 1. Gửi PUT /admin/drivers/{id}/verify | verified_status: maybe | HTTP 400 (chỉ chấp nhận verified/rejected) | Medium |
| TC-ADMU-014 | Quản trị khách hàng & tài xế | Gọi API admin khi không có token | Không đăng nhập | 1. Gửi GET /admin/customers không kèm token | (không có token) | HTTP 401 | High |

## Quản trị Báo cáo & Audit

| Test Case ID | Test Scenario | Test Case | Preconditions | Test Steps | Test Data | Expected Result | Priority |
|---|---|---|---|---|---|---|---|
| TC-ADMR-001 | Quản trị: báo cáo, phân quyền, audit | Xem báo cáo tổng hợp theo khoảng thời gian hợp lệ | Có dữ liệu Trip/Payment trong khoảng thời gian chọn | 1. Gửi GET /admin/reports?from=2026-01-01&to=2026-01-31 | from: 2026-01-01<br>to: 2026-01-31 | HTTP 200; trả về total_trips, total_revenue, completion_rate, cancellation_rate, top_drivers khớp dữ liệu thực tế | High |
| TC-ADMR-002 | Quản trị: báo cáo, phân quyền, audit | Xem báo cáo thiếu tham số from/to (required) | Đăng nhập operator | 1. Gửi GET /admin/reports không có from, to | (không có query param) | HTTP 400 (from, to là required theo openapi.yaml) | High |
| TC-ADMR-003 | Quản trị: báo cáo, phân quyền, audit | from lớn hơn to (khoảng ngày không hợp lệ) | Đăng nhập operator | 1. Gửi GET /admin/reports?from=2026-02-01&to=2026-01-01 | from: 2026-02-01<br>to: 2026-01-01 | HTTP 400 | Medium |
| TC-ADMR-004 | Quản trị: báo cáo, phân quyền, audit | Báo cáo cho khoảng thời gian không có dữ liệu | Không có Trip nào trong khoảng ngày chọn | 1. Gửi GET /admin/reports với khoảng ngày trống dữ liệu | from/to là khoảng ngày xa trong quá khứ | HTTP 200; total_trips=0, total_revenue=0, completion_rate/cancellation_rate = 0 (không lỗi chia cho 0) | Medium |
| TC-ADMR-005 | Quản trị: báo cáo, phân quyền, audit | Liệt kê tài khoản nhân viên vận hành | Đăng nhập bằng super_admin | 1. Gửi GET /admin/users | (super_admin token) | HTTP 200; trả về danh sách tài khoản operator/support_staff/super_admin | Medium |
| TC-ADMR-006 | Quản trị: báo cáo, phân quyền, audit | Tạo tài khoản nhân viên vận hành mới hợp lệ | Đăng nhập super_admin | 1. Gửi POST /admin/users | phone: 0977000001<br>full_name: 'NV Vận Hành A'<br>role: operator | HTTP 201; tài khoản mới được tạo với đúng role | High |
| TC-ADMR-007 | Quản trị: báo cáo, phân quyền, audit | Tạo tài khoản operator bằng tài khoản không phải super_admin | Đăng nhập bằng role=operator (không phải super_admin) | 1. Gửi POST /admin/users | phone: 0977000002<br>role: operator | HTTP 403 (QT-21: chỉ super_admin được tạo tài khoản quản trị) | High |
| TC-ADMR-008 | Quản trị: báo cáo, phân quyền, audit | Tạo tài khoản với số điện thoại đã tồn tại | phone đã tồn tại trong hệ thống | 1. Gửi POST /admin/users với phone trùng | phone: 0977000001 (đã tồn tại) | HTTP 409 | Medium |
| TC-ADMR-009 | Quản trị: báo cáo, phân quyền, audit | Phân quyền (đổi role) cho tài khoản nhân viên | Tài khoản userId tồn tại, đăng nhập super_admin | 1. Gửi PUT /admin/users/{id}/role | role: operator | HTTP 200; role được cập nhật | High |
| TC-ADMR-010 | Quản trị: báo cáo, phân quyền, audit | Phân quyền với role ngoài enum cho phép | Đăng nhập super_admin | 1. Gửi PUT /admin/users/{id}/role | role: super_hacker | HTTP 400 (vi phạm enum) | Medium |
| TC-ADMR-011 | Quản trị: báo cáo, phân quyền, audit | Tài khoản operator thường cố tự phân quyền cho chính mình lên super_admin | Đăng nhập role=operator | 1. Gửi PUT /admin/users/{id}/role (chính mình) | role: super_admin | HTTP 403 (không đủ quyền tự nâng cấp bản thân) | High |
| TC-ADMR-012 | Quản trị: báo cáo, phân quyền, audit | Xem audit log không có bộ lọc | Có nhiều audit log đã ghi nhận | 1. Gửi GET /admin/audit-logs | (không filter) | HTTP 200; trả về danh sách log, có admin_id, action, target_entity, target_id, created_at (QT-22) | High |
| TC-ADMR-013 | Quản trị: báo cáo, phân quyền, audit | Xem audit log lọc theo admin_id | Có log của nhiều admin khác nhau | 1. Gửi GET /admin/audit-logs?admin_id=xxx | admin_id: xxx | HTTP 200; chỉ trả về log của đúng admin_id đó | Medium |
| TC-ADMR-014 | Quản trị: báo cáo, phân quyền, audit | Kiểm tra audit log là bất biến (không có API sửa/xóa log) | Đã có ít nhất 1 audit log | 1. Thử tìm endpoint PUT/DELETE cho /admin/audit-logs (không tồn tại trong openapi.yaml) | (kiểm tra thiết kế, không phải gọi thực tế) | Xác nhận: hệ thống KHÔNG cung cấp API sửa/xóa audit log — đây là chủ đích thiết kế (audit log phải bất biến), không phải thiếu sót | High |
