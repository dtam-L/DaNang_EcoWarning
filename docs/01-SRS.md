# Đặc tả Yêu cầu Phần mềm (SRS) — DaNang EcoWarning

| Mục | Thông tin |
|-----|-----------|
| Dự án | DaNang EcoWarning — Hệ thống giám sát môi trường & cảnh báo rủi ro thiên tai |
| Phiên bản tài liệu | 1.0 |
| Tác giả | Đặng Văn Tâm |
| Trạng thái | Baseline theo hiện trạng hệ thống |

---

## 1. Giới thiệu

### 1.1 Bối cảnh & vấn đề
Đà Nẵng thường xuyên chịu ảnh hưởng của bão, lũ, ngập lụt và sạt lở. Dữ liệu môi trường, thủy văn, nông nghiệp và phòng chống thiên tai đã được công bố dưới dạng **dữ liệu mở**, nhưng:

- Dữ liệu **phân tán** ở nhiều file CSV, nhiều định dạng khác nhau, khó tra cứu và so sánh.
- Người dân **không có kênh trực quan** để biết vị trí nhà trú bão, trạm cảnh báo lũ, khu vực từng sạt lở gần mình.
- Thông tin sự cố tại hiện trường (ngập, cây đổ, cháy…) **chưa được thu thập có cấu trúc** từ cộng đồng.

### 1.2 Mục tiêu nghiệp vụ
| Mã | Mục tiêu | Chỉ số đo (KPI) |
|----|----------|-----------------|
| BO-01 | Tập trung dữ liệu môi trường – thiên tai – nông nghiệp vào một nơi | 26/26 bộ dữ liệu mở được nạp vào CSDL tập trung; tỷ lệ bản ghi nạp thành công (records inserted / processed) |
| BO-02 | Giúp người dùng nắm nhanh tình hình thời tiết & rủi ro | Xem được thời tiết hiện tại, dự báo 5 ngày, AQI trong 1 màn hình |
| BO-03 | Trực quan hóa hạ tầng phòng chống thiên tai trên bản đồ | 6 loại địa điểm hiển thị trên bản đồ, có hồ sơ chi tiết từng địa điểm |
| BO-04 | Thu thập báo cáo sự cố từ cộng đồng có cấu trúc, đáng tin cậy | 100% báo cáo hợp lệ có tọa độ trong Đà Nẵng, loại sự cố chuẩn hóa, trạng thái xử lý |
| BO-05 | Hỗ trợ phân tích tác động thiên tai & thời tiết lên nông nghiệp | Dashboard thiệt hại thiên tai theo năm và chỉ số nông nghiệp theo tiêu chí lọc |

### 1.3 Phạm vi
**Trong phạm vi (đã triển khai):**
1. Nạp và chuẩn hóa dữ liệu mở (module *Collector Data*).
2. Tra cứu, thống kê dữ liệu (module *Search & Statistics*).
3. Dashboard thống kê thiệt hại thiên tai và nông nghiệp.
4. Trang thời tiết (thời tiết hiện tại, dự báo, chất lượng không khí).
5. Bản đồ rủi ro & hạ tầng phòng chống thiên tai.
6. Gửi báo cáo sự cố từ người dân (module *Citizen Report*).

**Ngoài phạm vi phiên bản hiện tại (Planned):**
- Màn hình kiểm duyệt báo cáo cho cơ quan quản lý (chuyển trạng thái Verified/Rejected).
- Hiển thị báo cáo đã xác minh lên bản đồ rủi ro cộng đồng.
- Gửi thông báo đẩy / SMS cảnh báo.
- Đăng nhập, phân quyền người dùng.

### 1.4 Thuật ngữ
| Thuật ngữ | Giải thích |
|-----------|-----------|
| Asset (Địa điểm) | Đối tượng có thể định vị: hồ, sông, trạm đo mưa, nhà trú bão, khu vực sạt lở… |
| Metric (Chỉ số) | Loại số liệu đo: lượng mưa, nhiệt độ, sản lượng cây trồng, thiệt hại… |
| Observation (Quan sát) | Một giá trị của Metric tại một Asset, ở một thời điểm |
| Report (Báo cáo sự cố) | Thông tin sự cố do người dân gửi lên |
| AQI | Chỉ số chất lượng không khí |

---

## 2. Stakeholder & người dùng

| Nhóm | Vai trò / Nhu cầu chính | Mức ảnh hưởng | Mức quan tâm |
|------|------------------------|---------------|--------------|
| **Người dân** | Xem thời tiết, cảnh báo; tìm nhà trú bão / trạm cảnh báo gần nhà; báo cáo sự cố | Trung bình | Cao |
| **Cơ quan quản lý / Ban chỉ huy PCTT** | Nắm sự cố từ hiện trường, xác minh báo cáo, theo dõi thiệt hại theo năm | Cao | Cao |
| **Người làm nông nghiệp / nhà phân tích** | Theo dõi sản lượng, diện tích cây trồng, tác động thời tiết | Thấp | Trung bình |
| **Quản trị hệ thống** | Nạp dữ liệu, theo dõi nhật ký nạp dữ liệu, vận hành hệ thống | Cao | Trung bình |

Hệ thống bên ngoài: **Cổng dữ liệu mở Đà Nẵng** (nguồn CSV), **OpenWeather API** (thời tiết, AQI), **Goong Maps** (bản đồ), **Cloudinary** (lưu ảnh).

---

## 3. Yêu cầu chức năng

### 3.1 Module M1 — Nạp dữ liệu (Collector Data)
| Mã | Yêu cầu | Ưu tiên | Trạng thái |
|----|---------|---------|-----------|
| FR-01 | Hệ thống tự động nạp 26 bộ dữ liệu CSV thuộc 4 lĩnh vực khi khởi tạo lần đầu | Must | Implemented |
| FR-02 | Chuẩn hóa dữ liệu về mô hình chung Asset – Metric – Observation | Must | Implemented |
| FR-03 | Ghi nhật ký mỗi lần nạp: file, thời gian bắt đầu/kết thúc, trạng thái, số bản ghi xử lý / thêm mới, lỗi | Must | Implemented |
| FR-04 | Không nạp lặp nếu dữ liệu đã tồn tại | Must | Implemented |
| FR-05 | Bản ghi lỗi định dạng được bỏ qua và ghi log, không làm dừng toàn bộ quá trình nạp | Should | Implemented |

### 3.2 Module M2 — Dashboard thống kê
| Mã | Yêu cầu | Ưu tiên | Trạng thái |
|----|---------|---------|-----------|
| FR-06 | Biểu đồ chỉ số theo nhóm (Thời tiết, Nông nghiệp, Môi trường…), cho phép chọn nhóm và chỉ số | Must | Implemented |
| FR-07 | Biểu đồ thiệt hại thiên tai theo năm | Must | Implemented |
| FR-08 | Click vào một năm để xem chi tiết thiệt hại; click lại để trở về tổng cộng | Should | Implemented |
| FR-09 | Click một chỉ tiêu thiệt hại để xem lịch sử theo thời gian (popup) | Could | Implemented |
| FR-10 | Tra cứu nông nghiệp theo tiêu chí: đơn vị, loại cây trồng, khía cạnh (diện tích / sản lượng…) | Should | Implemented |

### 3.3 Module M3 — Thời tiết
| Mã | Yêu cầu | Ưu tiên | Trạng thái |
|----|---------|---------|-----------|
| FR-11 | Hiển thị thời tiết hiện tại tại Đà Nẵng (nhiệt độ, min/max, mô tả) | Must | Implemented |
| FR-12 | Dự báo theo giờ trong ngày và dự báo 5 ngày | Must | Implemented |
| FR-13 | Biểu đồ nhiệt độ – độ ẩm và lượng mưa dự kiến | Should | Implemented |
| FR-14 | Hiển thị chỉ số chất lượng không khí (AQI) và nồng độ các chất | Should | Implemented |

### 3.4 Module M4 — Bản đồ rủi ro
| Mã | Yêu cầu | Ưu tiên | Trạng thái |
|----|---------|---------|-----------|
| FR-15 | Chọn loại địa điểm (Khu vực sạt lở, Nhà trú bão, Hồ ao, Hồ thủy lợi, Trạm cảnh báo ven biển, Trạm đo mưa) kèm tỷ lệ % số lượng | Must | Implemented |
| FR-16 | Hiển thị các địa điểm thuộc loại đã chọn dưới dạng marker trên bản đồ | Must | Implemented |
| FR-17 | Click marker để mở hồ sơ địa điểm: thông tin chung và dữ liệu mới nhất | Must | Implemented |
| FR-18 | Tìm kiếm địa điểm không có tọa độ (sông…) theo tên, tối thiểu 2 ký tự | Could | Implemented |
| FR-19 | Hiển thị báo cáo sự cố đã xác minh trên bản đồ | Should | Planned |

### 3.5 Module M5 — Báo cáo sự cố cộng đồng
| Mã | Yêu cầu | Ưu tiên | Trạng thái |
|----|---------|---------|-----------|
| FR-20 | Người dân chọn 1 trong 11 loại sự cố thuộc 4 nhóm (Thời tiết, Nước, Cháy, Hạ tầng) | Must | Implemented |
| FR-21 | Chọn vị trí sự cố trên bản đồ, nhập địa chỉ và mô tả | Must | Implemented |
| FR-22 | Form hiển thị trường chi tiết động theo loại sự cố (xem BR-08) | Should | Implemented |
| FR-23 | Đính kèm 1 ảnh hiện trường (không bắt buộc) | Should | Implemented |
| FR-24 | Báo cáo mới được lưu với trạng thái **PENDING** | Must | Implemented |
| FR-25 | Cơ quan quản lý xem danh sách báo cáo và chuyển trạng thái VERIFIED / REJECTED | Must | Planned |

---

## 4. Quy tắc nghiệp vụ (Business Rules)

| Mã | Quy tắc | Nơi áp dụng |
|----|---------|-------------|
| BR-01 | Tọa độ báo cáo bắt buộc và phải nằm trong phạm vi Đà Nẵng: vĩ độ 15.90 – 16.30, kinh độ 107.80 – 108.40 | M5 |
| BR-02 | Loại sự cố phải thuộc danh mục chuẩn (11 mã: STORM, HIGH_WIND, LIGHTNING, FLOOD, FLASH_FLOOD, LANDSLIDE, FOREST_FIRE, URBAN_FIRE, FALLEN_TREE, POWER_LINE_DOWN, SEVERE_TRAFFIC_JAM) | M5 |
| BR-03 | Thời điểm bắt đầu sự cố bắt buộc, không ở tương lai, và không cũ hơn **7 ngày (168 giờ)** | M5 |
| BR-04 | Nếu có thời điểm kết thúc thì phải **sau** thời điểm bắt đầu | M5 |
| BR-05 | Mô tả tối đa 500 ký tự, không chứa mã HTML/script | M5 |
| BR-06 | Ảnh đính kèm tối đa **5 MB**, định dạng JPEG / PNG / WEBP | M5 |
| BR-07 | Mọi báo cáo mới đều ở trạng thái PENDING; chỉ cơ quan quản lý được chuyển sang VERIFIED hoặc REJECTED | M5 |
| BR-08 | Thông tin chi tiết theo loại: Ngập lụt → độ sâu (cm), loại ngập (ngoài đường / trong nhà); Cây đổ → có chắn đường không; Cháy rừng → có gần khu dân cư không; Kẹt xe → nguyên nhân, độ dài (km) | M5 |
| BR-09 | Dữ liệu chỉ được nạp khi CSDL chưa có dữ liệu (tránh trùng lặp) | M1 |
| BR-10 | Tên Asset và tên Metric là duy nhất | M1 |

---

## 5. Yêu cầu phi chức năng

| Mã | Nhóm | Yêu cầu |
|----|------|---------|
| NFR-01 | Bảo mật | Lọc nội dung mô tả để chống chèn mã độc (XSS); khóa API bên thứ ba lưu trong biến môi trường, không commit lên repo |
| NFR-02 | Toàn vẹn dữ liệu | Tạo báo cáo trong một transaction; nhật ký nạp dữ liệu có mã lần chạy (runId) để truy vết |
| NFR-03 | Khả năng mở rộng | Kiến trúc microservices (API Gateway, Service Discovery, Config Server) — mỗi module triển khai và mở rộng độc lập |
| NFR-04 | Khả dụng | Các service có health check; khởi chạy toàn bộ bằng Docker Compose |
| NFR-05 | Khả dụng (UX) | Giao diện tiếng Việt; hiển thị trạng thái đang tải và thông báo lỗi khi không lấy được dữ liệu |
| NFR-06 | Tài liệu hóa | API được mô tả tự động bằng Swagger / OpenAPI |

---

## 6. Giả định & ràng buộc
- Dữ liệu mở được cập nhật theo năm; hệ thống hiện nạp một lần khi khởi tạo (chưa có lịch cập nhật định kỳ).
- Dữ liệu thời tiết thời gian thực phụ thuộc OpenWeather API; bản đồ phụ thuộc Goong Maps.
- Chưa có đăng nhập: mọi người dùng đều có thể gửi báo cáo.

---

## 7. Gap analysis — hiện trạng so với yêu cầu

| # | Khoảng trống | Ảnh hưởng nghiệp vụ | Đề xuất |
|---|--------------|---------------------|---------|
| G-01 | Chưa có chức năng kiểm duyệt báo cáo (FR-25) | Báo cáo dừng ở PENDING, chưa tạo giá trị cho cơ quan quản lý | Xây màn hình kiểm duyệt + API cập nhật trạng thái, bổ sung vai trò Moderator |
| G-02 | Báo cáo chưa hiển thị trên bản đồ (FR-19) | Cộng đồng chưa thấy được sự cố xung quanh | Hiển thị báo cáo VERIFIED dưới dạng lớp bản đồ riêng |
| G-03 | API contract giữa giao diện và backend chưa thống nhất: giao diện gửi JSON (ảnh dạng base64, trường `description`), backend nhận multipart (`request` + `image`, trường `reportDescription`) | Có rủi ro mất mô tả / ảnh khi gửi báo cáo | Thống nhất đặc tả API (xem UC-05) và kiểm thử lại luồng gửi báo cáo |
| G-04 | Thời điểm sự cố được lấy tự động là thời điểm gửi | Không ghi nhận đúng sự cố xảy ra trước đó (trong 7 ngày cho phép) | Thêm trường chọn thời gian bắt đầu / kết thúc trên form |
| G-05 | Chưa có xác thực người dùng | Rủi ro báo cáo spam | Bổ sung đăng nhập / giới hạn tần suất gửi |
