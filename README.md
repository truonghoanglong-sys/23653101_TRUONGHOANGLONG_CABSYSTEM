# 23653101_TRUONGHOANGLONG_CABSYSTEM
**###BƯỚC 1: TÌM HIỂU**

**Lý do hệ thống cũ không đáp ứng được và phải xây dựng hệ thống mới:**
* Việc phân công tài xế chủ yếu được thực hiện thủ công, gây tốn thời gian và dễ sai sót.
* Khách hàng khó theo dõi trạng thái chuyến đi theo thời gian thực.
* Thông tin thanh toán chưa được quản lý tập trung, gây khó khăn cho việc đối soát.
* Bộ phận vận hành gặp khó khăn khi muốn mở rộng hệ thống do kiến trúc cũ không linh hoạt và không chịu được tải cao.
### Giải pháp của hệ thống mới 
* **Tự động hóa ghép chuyến (Smart Matching):** Tự động tìm tài xế gần nhất dựa trên GPS; tự động chuyển tiếp sang tài xế khác nếu tài xế đầu tiên từ chối/không phản hồi mà không bắt khách hàng đặt lại.
* **Kiến trúc mở rộng & Chịu lỗi:** Các module (Đặt xe, Thanh toán, Thông báo) hoạt động độc lập, tự động nâng cấp tài nguyên khi tải cao vào giờ cao điểm; sự cố ở module này không làm gián đoạn toàn bộ hệ thống.
* **Bảo mật thanh toán (Tokenization):** Tích hợp cổng thanh toán bên thứ ba an toàn, không lưu thông tin thẻ nhạy cảm trên hệ thống; tự động xử lý lại khi giao dịch thất bại.
* **Quản trị & Phân quyền chặt chẽ:** Áp dụng mô hình phân quyền RBAC cho nhân viên vận hành và ghi nhật ký thao tác (Audit Logs) để kiểm soát rủi ro.
* **Khả năng mở rộng tương lai:** Thiết kế linh hoạt, dễ dàng tích hợp thêm kênh thông báo, phương thức thanh toán hoặc các dịch vụ mới (xe ghép, giao hàng...) trong tương lai.
#### Tác nhân người dùng (User Actors)
* **Khách hàng (Customer):** Đăng ký/đăng nhập, tạo yêu cầu đặt xe, theo dõi vị trí tài xế real-time, thanh toán và đánh giá chất lượng dịch vụ.
* **Tài xế (Driver):** Quản lý hồ sơ/phương tiện, bật trạng thái sẵn sàng, nhận/từ chối chuyến, cập nhật tiến trình chuyến đi và định vị GPS.
* **Nhân viên Vận hành (Ops Staff):** Giám sát chuyến đi real-time, duyệt hồ sơ tài xế, hỗ trợ xử lý sự cố chuyến đi và tra cứu giao dịch.
* **Quản trị viên / Ban Giám đốc (Admin/Management):** Phân quyền hệ thống, kiểm tra Audit Log bảo mật và xem báo cáo doanh thu, hiệu suất vận hành.

####  Tác nhân hệ thống tích hợp (External Systems)
* **Cổng thanh toán (Payment Gateway):** Xử lý giao dịch thanh toán điện tử an toàn theo cơ chế Tokenization.
* **Hạ tầng thông báo (Notification Gateway):** Gửi SMS, Email và Push Notification tức thì đến thiết bị người dùng.
**###BƯỚC 2**
**2. Các bên liên quan**

| Stakeholder | Vai trò trong hệ thống |
| :--- | :--- |
| **Khách hàng** | Khởi tạo yêu cầu đặt xe, theo dõi chuyến đi real-time, thanh toán (tiền mặt/điện tử) và đánh giá dịch vụ. |
| **Tài xế** | Cập nhật hồ sơ/phương tiện, bật trạng thái sẵn sàng, nhận/từ chối chuyến và cập nhật tiến trình chuyến đi. |
| **Nhân viên Vận hành** | Giám sát danh sách chuyến đi/tài xế real-time, quản lý tài khoản và hỗ trợ xử lý các sự cố phát sinh. |
| **Nhân viên Tài chính** | Quản lý đối soát giao dịch thanh toán, theo dõi doanh thu/chiết khấu tài xế, xử lý hoàn tiền và tra cứu dữ liệu tài chính. |
| **Ban Giám đốc** | Xem báo cáo doanh thu & hiệu suất, quản lý phân quyền hệ thống và đưa ra các quyết định kinh doanh. |
| **Quản trị viên hệ thống** | Làm rõ quy tắc nghiệp vụ, thiết kế, xây dựng và triển khai hệ thống trong thời gian 7 tuần. |
| **Cổng thanh toán** | Tiếp nhận và xử lý giao dịch thanh toán trực tuyến . |
| **Nhà cung cấp Thông báo** | Chịu trách nhiệm truyền tải các thông báo (Push Notification/SMS/Email) tức thì đến thiết bị người dùng. |
3. Lập stackholder matric
```mermaid
quadrantChart
    title Ma trận Stakeholder - Hệ thống CAB
    x-axis Mức độ quan tâm thấp --> Mức độ quan tâm cao
    y-axis Mức độ ảnh hưởng thấp --> Mức độ ảnh hưởng cao
    quadrant-1 Quản lý chặt chẽ
    quadrant-2 Duy trì sự hài lòng
    quadrant-3 Theo dõi sát sao
    quadrant-4 Cung cấp thông tin
    "Ban Giám đốc": [0.85, 0.90]
    "Quản trị viên hệ thống": [0.90, 0.85]
    "Nhân viên Tài chính": [0.80, 0.75]
    "Cổng thanh toán": [0.30, 0.80]
    "Nhân viên Vận hành": [0.80, 0.40]
    "Khách hàng": [0.85, 0.30]
    "Tài xế": [0.85, 0.25]
    "Nhà cung cấp Thông báo": [0.25, 0.20]
```
### Bước 3: Mục đích kinh doanh (Business Purpose & Goals)

* **Tự động hóa vận hành:** Tự động ghép chuyến thông minh qua vị trí GPS, loại bỏ hoàn toàn quy trình phân công thủ công, tối ưu chi phí nhân sự tổng đài.
* **Đa dạng phương thức thanh toán:** Hỗ trợ linh hoạt song song cả **Thanh toán tiền mặt** và **Thanh toán trực tuyến** (Ví điện tử/Thẻ) an toàn qua cơ chế Tokenization.
* **Xử lý truy cập số lượng lớn:** Đảm bảo hệ thống chịu tải cao, phục vụ hàng nghìn khách hàng và tài xế truy cập đồng thời ổn định vào các giờ cao điểm.
* **Nâng cao trải nghiệm khách hàng:** Minh bạch cước phí, theo dõi vị trí xe real-time, nhận thông báo tức thì và đánh giá chất lượng dịch vụ sau chuyến.
* **Tối ưu hiệu suất tài xế:** Giúp tài xế chủ động bật/tắt nhận chuyến, giảm thời gian di chuyển rỗng nhờ nhận các chuyến đi ở vị trí gần nhất.
* **Quản trị tài chính & Kiểm soát rủi ro:** Tập trung hóa dữ liệu giao dịch, tự động xử lý lại khi thanh toán thất bại, ghi log theo vết (Audit Log) và phân quyền truy cập chặt chẽ (RBAC).
* **Công cụ vận hành real-time:** Trang bị Dashboard quản trị cho nhân viên vận hành giám sát chuyến đi, quản lý tài khoản và can thiệp hỗ trợ sự cố kịp thời.
* **Chịu lỗi & Hoạt động liên tục (Fault Tolerance):** Đảm bảo sự cố từ các dịch vụ bên thứ ba (thanh toán, thông báo) không làm ngừng trệ chức năng đặt xe cốt lõi.
* **Hỗ trợ báo cáo & Quyết định kinh doanh:** Cung cấp báo cáo trực quan cho Ban Giám đốc về doanh thu, tỷ lệ hoàn thành/hủy chuyến và KPI hiệu suất tài xế.
* **Sẵn sàng mở rộng tương lai:** Xây dựng kiến trúc linh hoạt để dễ dàng bổ sung thêm dịch vụ mới (giao hàng, xe ghép...), kênh thông báo hoặc phương thức thanh toán mới.
  
**###Bước 4: Phạm vi dự án (Project Scope - 7 Tuần)**

#### 1. Trong phạm vi (In-Scope)

* **Xác thực & Phân quyền cơ bản (Authentication & RBAC):**
  * Đăng ký, đăng nhập an toàn cho người dùng (Khách hàng, Tài xế).
  * Phân quyền truy cập theo vai trò (Khách hàng, Tài xế, Nhân viên Vận hành, Nhân viên Tài chính, Ban Giám đốc).

* **Thiết kế modul hóa (Modular Design):**
  * **Modul Quản lý Khách hàng (Customer Management):** Quản lý thông tin hồ sơ khách hàng, lịch sử chuyến đi, thông tin thanh toán lưu trữ và lịch sử đánh giá/phản hồi dịch vụ.
  * **Modul Tài khoản & Xác thực (Auth):** Quản lý đăng nhập, phân quyền và bảo mật phiên làm việc.
  * **Modul Đặt xe (Booking):** Khởi tạo chuyến đi, tự động tìm tài xế gần nhất qua GPS và xử lý chuyển tiếp khi tài xế từ chối/hết giờ.
  * **Modul Định vị (Tracking):** Theo dõi vị trí tài xế và trạng thái chuyến đi theo thời gian thực trên bản đồ.
  * **Modul Thanh toán (Payment):** Hỗ trợ song song **Tiền mặt** và **Thanh toán trực tuyến** (môi trường thử nghiệm Sandbox).
  * **Modul Quản trị (Dashboard):** Giao diện hỗ trợ cho Nhân viên Vận hành (giám sát), Nhân viên Tài chính (xem giao dịch, đối soát) và Ban Giám đốc (xem báo cáo doanh thu).

#### 2. Ngoài phạm vi (Out-of-Scope)
* Tích hợp cổng thanh toán thực tế (chỉ dùng tài khoản thử nghiệm Sandbox).
* Các tính năng nâng cao: Đặt xe ghép, giao hàng, đặt trước chuyến đi.
* Chương trình tích điểm, khuyến mãi hoặc mã giảm giá phức tạp.

#### 3. Lộ trình 7 tuần (7-Week Roadmap)

| Tuần | Công việc chính |
| :---: | :--- |
| **1** | Thu thập yêu cầu, xác định phạm vi và lập bảng Stakeholder. |
| **2** | Phân tích nghiệp vụ (Sơ đồ Use Case, Activity Diagram). |
| **3** | Thiết kế Cơ sở dữ liệu (ERD) và phân chia cấu trúc các Modul. |
| **4** | Xây dựng Modul Auth (Đăng nhập/Phân quyền), Modul Quản lý Khách hàng & Modul Đặt xe. |
| **5** | Xây dựng Modul Thanh toán (Tiền mặt/Online), Tracking & Giao diện Dashboard Quản trị. |
| **6** | Kiểm thử tích hợp giữa các modul và sửa lỗi. |
| **7** | Hoàn thiện tài liệu README.md và tổng kết báo cáo dự án. |

### Bước 5: Yêu cầu nghiệp vụ (Business Requirements)

#### Bảng Quy trình Nghiệp vụ

| Mã BR | Yêu cầu Nghiệp vụ cốt lõi | KPI & Tiêu chí Nghiệm thu cụ thể |
| :---: | :--- | :--- |
| **BR-01** | **Điều phối & Ghép chuyến thông minh (Smart Matching)** | • **100%** luồng phân công chạy tự động dựa trên tọa độ GPS, loại bỏ hoàn toàn quy trình can thiệp thủ công từ tổng đài.<br>• Hệ thống tự động chuyển tiếp yêu cầu sang tài xế kế cận khi tài xế trước từ chối hoặc hết thời gian phản hồi (timeout).<br>• Đảm bảo trải nghiệm liền mạch: Giữ nguyên dữ liệu chuyến đi, **không bắt khách hàng phải thao tác đặt lại chuyến**. |
| **BR-02** | **Định vị hành trình & Truyền tải thông báo tức thì** | • Cho phép Khách hàng giám sát vị trí tài xế và tiến trình chuyến đi theo thời gian thực trên bản đồ (Modul Tracking).<br>• Tự động kích hoạt **100%** thông báo đa kênh (**Push Notification/SMS/Email**) qua Notification Gateway đến thiết bị người dùng theo từng mốc chuyến đi.<br>• Thu thập phản hồi và đánh giá chất lượng dịch vụ của khách hàng ngay sau khi kết thúc hành trình. |
| **BR-03** | **Quản lý Cước phí & Tích hợp Thanh toán An toàn (Tokenization)** | • Tích hợp linh hoạt hai phương thức: **Thanh toán tiền mặt** và **Thanh toán trực tuyến** (kết nối Cổng thanh toán bên thứ ba qua môi trường thử nghiệm Sandbox).<br>• Tuân thủ an toàn dữ liệu: **0%** lưu trữ thông tin thẻ nhạy cảm (CVV, số thẻ đầy đủ) trên hệ thống CAB nhờ cơ chế Tokenization.<br>• Hỗ trợ tự động xử lý lại (Retry) giao dịch điện tử hoặc cho phép chuyển sang tiền mặt khi thanh toán thất bại. |
| **BR-04** | **Phân quyền truy cập RBAC & Giám sát Vận hành Real-time** | • Thiết lập mô hình phân quyền **RBAC** chặt chẽ cho 5 nhóm tác nhân (*Khách hàng, Tài xế, Nhân viên Vận hành, Nhân viên Tài chính, Ban Giám đốc/Admin*).<br>• Trang bị Dashboard cho Nhân viên Vận hành giám sát các chuyến đi real-time, tra cứu lịch sử và hỗ trợ xử lý sự cố phát sinh.<br>• **100%** các thao tác thay đổi dữ liệu hoặc phân quyền hệ thống phải được ghi lại nhật ký vết (**Audit Logs**) để quản trị rủi ro. |
| **BR-05** | **Quản lý Tài chính, Đối soát & Báo cáo Quản trị** | • Cung cấp công cụ cho Nhân viên Tài chính thực hiện tra cứu giao dịch, đối soát doanh thu/chiết khấu tài xế và xử lý yêu cầu hoàn tiền.<br>• Xuất báo cáo trực quan cho Ban Giám đốc về các chỉ số vận hành: *Doanh thu tổng, số lượng chuyến đi, tỷ lệ hoàn thành/hủy chuyến, KPI hiệu suất tài xế*. |
| **BR-06** | **Thiết kế Modul hóa & Tối ưu Khả năng Chịu lỗi (Fault Tolerance)** | • Kiến trúc phân chia Modul độc lập (*Auth, Customer, Booking, Tracking, Payment, Dashboard*).<br>• **Đảm bảo tính hoạt động liên tục**: Sự cố từ các dịch vụ bên thứ ba (Cổng thanh toán, Hạ tầng thông báo) không làm ngừng trệ chức năng Đặt xe cốt lõi.<br>• Đáp ứng khả năng chịu tải cao vào giờ cao điểm và sẵn sàng mở rộng các dịch vụ mới (*giao hàng, xe ghép...*) trong tương lai. |

### Bước 6: Yêu cầu Chức năng Hệ thống (Functional Requirements - FR)
#### 1. Khách hàng (Customer)
* **FR-CUS-01 (Đăng ký / Đăng nhập):** Đăng ký tài khoản mới và xác thực đăng nhập để truy cập hệ thống.
* **FR-CUS-02 (Quản lý hồ sơ):** Xem, cập nhật thông tin cá nhân (Họ tên, Email, Ảnh đại diện) và quản lý danh sách địa chỉ yêu thích.
* **FR-CUS-03 (Đặt xe):** Nhập điểm đi, điểm đến, chọn loại xe, xem cước phí dự kiến và gửi yêu cầu tìm tài xế.
* **FR-CUS-04 (Hủy xe):** Chủ động hủy yêu cầu đặt xe hoặc hủy chuyến trước khi tài xế đón.
* **FR-CUS-05 (Thanh toán):** Chọn phương thức thanh toán (Tiền mặt / Online) và thực hiện giao dịch cho chuyến đi.
* **FR-CUS-06 (Đánh giá dịch vụ):** Chấm điểm số sao (1–5 sao) và gửi nhận xét về chất lượng chuyến đi sau khi hoàn tất.

#### 2. Tài xế (Driver)
* **FR-DRI-01 (Quản lý hồ sơ & Phương tiện):** Cập nhật thông tin cá nhân và tải lên giấy tờ xe (Bằng lái, Biển số, Cavet, Đăng kiểm) để chờ duyệt.
* **FR-DRI-02 (Cập nhật trạng thái sẵn sàng):** Chủ động bật/tắt chế độ nhận chuyến (Sẵn sàng / Ngừng nhận chuyến).
* **FR-DRI-03 (Xử lý yêu cầu chuyến đi):** Nhận thông báo chuyến đi mới và thực hiện Chấp nhận hoặc Từ chối trong thời gian quy định.
* **FR-DRI-04 (Cập nhật tiến trình đi):** Cập nhật trạng thái thực tế của chuyến đi theo luồng: *Đã đến điểm đón -> Bắt đầu di chuyển -> Hoàn thành chuyến đi*.
* **FR-DRI-05 (Xác nhận thu tiền mặt):** Xác nhận đã thu đủ số tiền mặt từ khách hàng đối với các chuyến đi thanh toán bằng tiền mặt.

#### 3. Nhân viên Vận hành (Ops Staff)
* **FR-OPS-01 (Duyệt hồ sơ tài xế):** Kiểm tra thông tin, hình ảnh giấy tờ do tài xế cung cấp để Phê duyệt hoặc Từ chối cấp quyền hoạt động.
* **FR-OPS-02 (Giám sát vận hành):** Theo dõi danh sách các chuyến đi đang diễn ra và vị trí/trạng thái của tài xế real-time trên bản đồ.
* **FR-OPS-03 (Can thiệp sự cố):** Can thiệp hủy chuyến, điều lại xe hoặc xử lý các sự cố phát sinh trong quá trình vận hành.

#### 4. Nhân viên Tài chính (Finance Staff)
* **FR-FIN-01 (Tra cứu giao dịch):** Khai thác và kiểm tra chi tiết thông tin lịch sử các giao dịch thanh toán trên hệ thống.
* **FR-FIN-02 (Đối soát tài chính):** Đối soát dữ liệu thanh toán giữa CSDL hệ thống và Cổng thanh toán để phát hiện, xử lý các giao dịch chênh lệch.
* **FR-FIN-03 (Quản lý ví tài xế):** Quản lý số dư, lịch sử biến động nguồn tiền, khấu trừ chiết khấu và công nợ trên ví của tài xế.

#### 5. Ban Giám đốc (Management)
* **FR-MGT-01 (Xem báo cáo doanh thu):** Trích xuất và theo dõi các chỉ số doanh thu tổng quan, doanh thu theo thời gian và phương thức thanh toán.
* **FR-MGT-02 (Xem báo cáo hiệu suất):** Theo dõi các báo cáo chỉ số KPI, tỷ lệ hoàn thành/hủy chuyến và hiệu suất hoạt động của tài xế.

#### 6. Quản trị viên Hệ thống (System Admin)
* **FR-ADM-01 (Cập nhật hệ thống):** Cấu hình tham số vận hành, cập nhật tính năng và bảo trì các thiết lập chung của hệ thống.
* **FR-ADM-03 (Sao lưu và lưu trữ):** Thực hiện sao lưu dữ liệu định kỳ, lưu trữ nhật ký thao tác (Audit Log) và khôi phục dữ liệu khi cần.

#### 7. Hệ thống Tích hợp bên ngoài (External Systems)
* **FR-EXT-01 (Cổng thanh toán - Xử lý giao dịch online):** Tiếp nhận và xử lý các giao dịch thanh toán trực tuyến qua Thẻ/Ví điện tử an toàn theo cơ chế Tokenization.
* **FR-EXT-02 (Nhà cung cấp Thông báo - Gửi thông báo):** Chịu trách nhiệm truyền tải các Push Notification, SMS, Email tức thì đến thiết bị của người dùng.

Bước 7: vẽ usecase tổng quát
<img width="973" height="977" alt="image" src="https://github.com/user-attachments/assets/3b1dea0d-104a-4000-a6ef-0fa5c6b955c0" />



Bước 8: đặc tả usecase
**###KHÁCH HÀNG**
**Đặc tả usecase Đăng ký**
## Use case: Đăng ký

| Thuộc tính | Nội dung |
| :--- | :--- |
| **– Tên use case:** | Đăng ký |
| **– Mô tả sơ lược:** | Cho phép Khách hàng tạo tài khoản mới trên hệ thống CAB System bằng số điện thoại/email để có thể sử dụng dịch vụ đặt xe. |
| **– Actor chính:** | Khách hàng |
| **– Actor phụ:** | Nhà cung cấp Thông báo (gửi mã OTP xác thực) |
| **– Tiền điều kiện (Pre-condition):** | Khách hàng chưa có tài khoản được đăng ký bằng số điện thoại/email trên hệ thống. |
| **– Hậu điều kiện (Post-condition):** | Tài khoản Khách hàng được tạo và kích hoạt thành công, sẵn sàng đăng nhập sử dụng. |

#### – Luồng sự kiện chính (main flow):

| Actor: Khách hàng | System |
| :--- | :--- |
| **1.** Chọn chức năng "Đăng ký" | |
| | **2.** Hiển thị form đăng ký gồm: Họ tên, Số điện thoại, Email, Mật khẩu, Xác nhận mật khẩu |
| **3.** Nhập đầy đủ thông tin và nhấn nút "Đăng ký" | |
| | **4.** Kiểm tra định dạng dữ liệu đầu vào (số điện thoại, email, độ mạnh mật khẩu) |
| | **5.** Kiểm tra số điện thoại/email chưa tồn tại trong hệ thống |
| | **6.** Gửi mã OTP xác thực đến số điện thoại/email đăng ký |
| **7.** Nhập mã OTP và nhấn "Xác nhận" | |
| | **8.** Xác thực mã OTP hợp lệ, tạo tài khoản, hiển thị thông báo đăng ký thành công. Kết thúc use case |

#### – Luồng sự kiện thay thế (alternate flow):

| Actor: Khách hàng | System |
| :--- | :--- |
| | **4.1.** Dữ liệu sai định dạng (ví dụ: số điện thoại không hợp lệ, mật khẩu không đủ mạnh), hiển thị thông báo lỗi tương ứng |
| **4.2.** Nhập lại thông tin, quay lại bước 3 | |
| | **5.1.** Số điện thoại/Email đã tồn tại trên hệ thống, hiển thị thông báo "Số điện thoại/Email đã được sử dụng" |
| **5.2.** Nhập lại thông tin khác, quay lại bước 3 | |
| | **8.1.** Mã OTP không đúng hoặc đã hết hạn, hiển thị thông báo "Mã OTP không chính xác hoặc đã hết hạn" |
| **8.2.** Chọn nhập lại mã OTP hoặc nhấn "Gửi lại mã OTP" | **8.3.** Nếu Khách hàng chọn nhập lại → quay lại bước 7; nếu Khách hàng chọn "Gửi lại mã OTP" → gửi mã OTP mới, quay lại bước 7 |

#### – Luồng sự kiện ngoại lệ (exception flow):

| Actor: Khách hàng | System |
| :--- | :--- |
| | **9.1.** Mất kết nối Internet trong quá trình gửi thông tin đăng ký, gửi OTP hoặc xác thực OTP, hiển thị thông báo "Mất kết nối, vui lòng kiểm tra Internet và thử lại" |
| **9.2.** Nhấn "OK", kiểm tra kết nối và thực hiện lại từ bước tương ứng | |

**Ghi chú:** Chưa quy định số lần giới hạn gửi lại OTP trong đặc tả BR hiện tại — nếu sau này viết test case với boundary value, cần bổ sung quy tắc số lần gửi lại tối đa vào bảng BR trước.

### Bước 9: quy trình nghiệp vụ business process
### QUY TRÌNH 1: ĐẶT XE VÀ TỰ ĐỘNG GHÉP CHUYẾN
<img width="926" height="1106" alt="image" src="https://github.com/user-attachments/assets/1f7747a1-a666-4a84-bc58-fc00ca01fbd0" />

### QUY TRÌNH 2: THỰC HIỆN VÀ HOÀN THÀNH CHUYẾN ĐI
<img width="1156" height="1246" alt="image" src="https://github.com/user-attachments/assets/d50b4f0e-634e-4107-b072-7091ad82159e" />


### DUYỆT HỒ SƠ TÀI XẾ & TÍCH HỢP HỆ THỐNG
<img width="1096" height="1166" alt="image" src="https://github.com/user-attachments/assets/9d24761d-4810-46f9-aba9-8c83b7410193" />


### BƯỚC 10: PHÂN TÍCH QUY TẮC NGHIỆP VỤ (BUSINESS RULES ANALYSIS)

| Nhóm Quy tắc | Tên Quy tắc Nghiệp vụ | Nội dung Chi tiết Đặc tả |
| :--- | :--- | :--- |
| **Ghép chuyến tự động** | Trạng thái Khả dụng | • Chỉ những Tài xế đang ở trạng thái **"Trực tuyến"** (Online) và **"Sẵn sàng"** (Available) mới được hệ thống ưu tiên phát thông báo mời chuyến. |
| **Ghép chuyến tự động** | Thuật toán Định vị (GPS) | • Hệ thống tự động quét và ưu tiên gửi đơn đặt xe cho Tài xế ở gần vị trí điểm đón của Khách hàng nhất trong bán kính tối đa 3km. |
| **Ghép chuyến tự động** | Thời gian Chờ (Timeout) | • Tài xế có tối đa 15 giây để nhấn **"Nhận chuyến"**. Quá 15 giây không phản hồi, hệ thống tự động coi là Từ chối và chuyển chuyến đi cho Tài xế tiếp theo mà không bắt Khách hàng đặt lại. |
| **Vận hành & Hồ sơ** | Điều kiện Hoạt động | • Tài xế chỉ được phép bật trạng thái **"Trực tuyến"** khi hồ sơ cá nhân và phương tiện (Bằng lái, Cavet, Đăng kiểm) đã được Nhân viên Vận hành phê duyệt. |
| **Đặt & Hủy chuyến** | Phí Hủy chuyến | • Khách hàng được miễn phí hủy chuyến trong vòng 2 phút đầu sau khi ghép xế thành công. Nếu hủy sau 2 phút, hệ thống sẽ ghi nhận phí phạt hủy chuyến vào đơn đặt tiếp theo. |
| **Thanh toán & Cước phí** | Khấu trừ Chiết khấu | • Hệ thống tự động trừ % hoa hồng chiết khấu dịch vụ trực tiếp vào Ví tài xế ngay khi chuyến đi hoàn thành thành công. |
| **Đánh giá & Khóa tài khoản** | Tỷ lệ Hoàn thành & Điểm Sao | • Tài xế có điểm đánh giá trung bình dưới 4.0 hoặc tỷ lệ hủy chuyến quá 15% trong tuần sẽ bị hệ thống tự động tạm khóa quyền nhận chuyến. |

--------------
### ĐẶC TẢ API
# Tài liệu API System

Tài liệu API của hệ thống được xây dựng theo chuẩn **OpenAPI 3.0.0** và được kiểm tra tính hợp lệ bằng **Swagger Editor**. 

Tất cả các tệp cấu hình API được phân chia chi tiết theo từng nhóm chức năng nghiệp vụ và lưu trữ tại thư mục `API/`.

---

## Danh sách API theo Nhóm chức năng

### 1. Khách hàng
| STT | Chức năng | Method / Thao tác | Tài liệu API |
| :---: | :--- | :--- | :--- |
| **1** | Đăng ký và Đăng nhập | `POST` đăng ký đăng nhập | `01_Đăng ký_Đăng nhập_Khách hàng.yaml` |
| **2** | Quản lý hồ sơ | `GET`, `POST`, `PUT`, `DELETE` quản lý hồ sơ | `02_Quản lý hồ sơ_Khách hàng.yaml` |
| **3** | Đặt xe | `POST` đặt xe | `03_Đặt xe_Khách hàng.yaml` |
| **4** | Hủy xe | `POST` hủy xe | `04_Hủy xe_Khách hàng.yaml` |
| **5** | Thanh toán | `POST` thanh toán | `05_Thanh toán_Khách hàng.yaml` |
| **6** | Đánh giá dịch vụ | `POST` đánh giá dịch vụ | `06_Đánh giá dịch vụ_Khách hàng.yaml` |

### 2. Tài xế
| STT | Chức năng | Method / Thao tác | Tài liệu API |
| :---: | :--- | :--- | :--- |
| **1** | Quản lý hồ sơ & Phương tiện | `GET`, `POST`, `PUT`, `DELETE` quản lý hồ sơ và phương tiện tài xế | `07_Quản lý hồ sơ & Phương tiện_Tài xế.yaml` |
| **2** | Cập nhật trạng thái sẵn sàng | `PUT` cập nhật trạng thái sẵn sàng | `08_Cập nhật trạng thái sẵn sàng_Tài xế.yaml` |
| **3** | Xử lý yêu cầu chuyến đi | `POST` xử lý yêu cầu chuyến đi | `09_Xử lý yêu cầu chuyến đi_Tài xế.yaml` |
| **4** | Cập nhật tiến trình đi | `PUT` cập nhật tiến trình đi | `10_Cập nhật tiến trình đi_Tài xế.yaml` |
| **5** | Xác nhận thu tiền mặt | `POST` xác nhận thu tiền mặt | `11_Xác nhận thu tiền mặt_Tài xế.yaml` |

### 3. Nhân viên Vận hành
| STT | Chức năng | Method / Thao tác | Tài liệu API |
| :---: | :--- | :--- | :--- |
| **1** | Duyệt hồ sơ tài xế | `POST` duyệt hồ sơ tài xế | `12_Duyệt hồ sơ tài xế_Nhân viên Vận hành.yaml` |
| **2** | Giám sát vận hành | `GET` giám sát vận hành | `13_Giám sát vận hành_Nhân viên Vận hành.yaml` |
| **3** | Can thiệp sự cố | `POST` can thiệp sự cố | `14_Can thiệp sự cố_Nhân viên Vận hành.yaml` |

### 4. Nhân viên Tài chính
| STT | Chức năng | Method / Thao tác | Tài liệu API |
| :---: | :--- | :--- | :--- |
| **1** | Tra cứu giao dịch | `GET` tra cứu giao dịch | `15_Tra cứu giao dịch_Nhân viên Tài chính.yaml` |
| **2** | Đối soát tài chính | `POST` đối soát tài chính | `16_Đối soát tài chính_Nhân viên Tài chính.yaml` |
| **3** | Quản lý ví tài xế | `GET`, `POST`, `PUT`, `DELETE` quản lý ví tài xế | `17_Quản lý ví tài xế_Nhân viên Tài chính.yaml` |

### 5. Ban Giám đốc
| STT | Chức năng | Method / Thao tác | Tài liệu API |
| :---: | :--- | :--- | :--- |
| **1** | Xem báo cáo doanh thu | `GET` xem báo cáo doanh thu | `18_Xem báo cáo doanh thu_Ban Giám đốc.yaml` |
| **2** | Xem báo cáo hiệu suất | `GET` xem báo cáo hiệu suất | `19_Xem báo cáo hiệu suất_Ban Giám đốc.yaml` |

### 6. Quản trị viên Hệ thống
| STT | Chức năng | Method / Thao tác | Tài liệu API |
| :---: | :--- | :--- | :--- |
| **1** | Cập nhật hệ thống | `GET`, `POST`, `PUT`, `DELETE` cập nhật hệ thống | `20_Cập nhật hệ thống_Quản trị viên Hệ thống.yaml` |
| **2** | Sao lưu và lưu trữ | `POST` sao lưu và lưu trữ | `21_Sao lưu và lưu trữ_Quản trị viên Hệ thống.yaml` |

### 7. Hệ thống Tích hợp bên ngoài
| STT | Chức năng | Method / Thao tác | Tài liệu API |
| :---: | :--- | :--- | :--- |
| **1** | Cổng thanh toán - Xử lý giao dịch online | `POST` cổng thanh toán | `22_Cổng thanh toán - Xử lý giao dịch online_Hệ thống Tích hợp bên ngoài.yaml` |
| **2** | Nhà cung cấp Thông báo - Gửi thông báo | `POST` nhà cung cấp thông báo | `23_Nhà cung cấp Thông báo - Gửi thông báo_Hệ thống Tích hợp bên ngoài.yaml` |

---

## Công cụ và Tiêu chuẩn

* **OpenAPI 3.0.0:** Tiêu chuẩn cấu trúc mô tả API.
* **YAML:** Định dạng tập tin được sử dụng để định nghĩa chi tiết các API.
* **Swagger Editor:** Công cụ chính được áp dụng để kiểm thử (validate) và trực quan hóa giao diện API.
* **HTTP Methods:** `GET`, `POST`, `PUT`, `DELETE`.
* **HTTP Status Codes:** `200` (OK), `201` (Created), `400` (Bad Request), `401` (Unauthorized), `403` (Forbidden), `404` (Not Found), `500` (Internal Server Error) tùy theo từng ngữ cảnh xử lý.
* **Authentication:** Sử dụng các cơ chế xác thực tiêu chuẩn phù hợp với từng phân quyền, bao gồm mã hóa **Bearer Token / JWT** đối với các API yêu cầu đăng nhập.


