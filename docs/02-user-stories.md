# User Stories & Product Backlog — DaNang EcoWarning

Độ ưu tiên theo **MoSCoW**: **M**ust have · **S**hould have · **C**ould have · **W**on't have (this release).
Trạng thái: ✅ Implemented · 🕒 Planned.

## 1. Epic & User Stories

### Epic E1 — Dữ liệu tập trung
| ID | User story | MoSCoW | Trạng thái | Yêu cầu |
|----|-----------|--------|-----------|---------|
| US-01 | Là **quản trị hệ thống**, tôi muốn hệ thống tự động nạp toàn bộ dữ liệu mở khi khởi tạo, để không phải nhập tay từng file | M | ✅ | FR-01, FR-02, FR-04 |
| US-02 | Là **quản trị hệ thống**, tôi muốn xem nhật ký từng lần nạp (số bản ghi xử lý / thành công / lỗi), để kiểm soát chất lượng dữ liệu | M | ✅ | FR-03, FR-05 |

### Epic E2 — Dashboard & phân tích
| ID | User story | MoSCoW | Trạng thái | Yêu cầu |
|----|-----------|--------|-----------|---------|
| US-03 | Là **cán bộ quản lý**, tôi muốn xem thiệt hại do thiên tai theo từng năm, để đánh giá xu hướng và lập kế hoạch phòng chống | M | ✅ | FR-07 |
| US-04 | Là **cán bộ quản lý**, tôi muốn xem chi tiết thiệt hại của một năm cụ thể, để biết hạng mục nào chịu thiệt hại lớn nhất | S | ✅ | FR-08, FR-09 |
| US-05 | Là **nhà phân tích nông nghiệp**, tôi muốn lọc dữ liệu theo loại cây trồng và khía cạnh (diện tích, sản lượng), để đánh giá tình hình sản xuất | S | ✅ | FR-06, FR-10 |

### Epic E3 — Thời tiết
| ID | User story | MoSCoW | Trạng thái | Yêu cầu |
|----|-----------|--------|-----------|---------|
| US-06 | Là **người dân**, tôi muốn xem thời tiết hiện tại và dự báo 5 ngày, để chủ động sinh hoạt và phòng tránh | M | ✅ | FR-11, FR-12, FR-13 |
| US-07 | Là **người dân**, tôi muốn biết chất lượng không khí hôm nay, để bảo vệ sức khỏe | S | ✅ | FR-14 |

### Epic E4 — Bản đồ rủi ro
| ID | User story | MoSCoW | Trạng thái | Yêu cầu |
|----|-----------|--------|-----------|---------|
| US-08 | Là **người dân**, tôi muốn xem vị trí nhà trú bão trên bản đồ, để biết nơi sơ tán gần nhất | M | ✅ | FR-15, FR-16 |
| US-09 | Là **người dân**, tôi muốn xem các khu vực từng xảy ra sạt lở, để tránh đi qua khi mưa lớn | M | ✅ | FR-15, FR-16 |
| US-10 | Là **người dùng**, tôi muốn click vào một địa điểm để xem thông tin và số liệu mới nhất | M | ✅ | FR-17 |
| US-11 | Là **người dùng**, tôi muốn tìm sông / hồ theo tên | C | ✅ | FR-18 |
| US-12 | Là **người dân**, tôi muốn thấy các sự cố đã được xác minh quanh mình trên bản đồ | S | 🕒 | FR-19 |

### Epic E5 — Báo cáo sự cố cộng đồng
| ID | User story | MoSCoW | Trạng thái | Yêu cầu |
|----|-----------|--------|-----------|---------|
| US-13 | Là **người dân**, tôi muốn gửi báo cáo sự cố kèm vị trí trên bản đồ, để cơ quan chức năng nắm được tình hình | M | ✅ | FR-20, FR-21, FR-24 |
| US-14 | Là **người dân**, tôi muốn đính kèm ảnh hiện trường, để báo cáo đáng tin cậy hơn | S | ✅ | FR-23 |
| US-15 | Là **người dân**, tôi muốn form chỉ hỏi những thông tin liên quan đến loại sự cố tôi chọn, để gửi nhanh | S | ✅ | FR-22 |
| US-16 | Là **cán bộ quản lý**, tôi muốn xem danh sách báo cáo đang chờ và xác minh / từ chối, để chỉ thông tin chính xác được công bố | M | 🕒 | FR-25 |
| US-17 | Là **người dân**, tôi muốn nhận thông báo đẩy khi có cảnh báo khẩn cấp | W | 🕒 | — |

## 2. Tiêu chí chấp nhận (Acceptance Criteria) — các story chính

### US-13 — Gửi báo cáo sự cố
- **AC1** — *Given* tôi đang ở trang Báo cáo, *when* tôi chọn loại "Ngập lụt", chọn vị trí trong Đà Nẵng, nhập mô tả và bấm Gửi, *then* hệ thống lưu báo cáo với trạng thái **PENDING** và hiển thị "Báo cáo đã được gửi thành công".
- **AC2** — *Given* vị trí nằm ngoài phạm vi Đà Nẵng (BR-01), *when* tôi bấm Gửi, *then* hệ thống từ chối và báo lỗi tọa độ không hợp lệ.
- **AC3** — *Given* mô tả dài hơn 500 ký tự hoặc chứa mã HTML (BR-05), *then* hệ thống từ chối báo cáo.
- **AC4** — *Given* gửi thành công, *then* form được làm mới về giá trị mặc định.

### US-14 — Đính kèm ảnh
- **AC1** — *Given* ảnh JPEG/PNG/WEBP ≤ 5 MB, *then* ảnh được lưu và gắn đường dẫn vào báo cáo.
- **AC2** — *Given* ảnh > 5 MB hoặc sai định dạng (BR-06), *then* hệ thống báo lỗi và không lưu báo cáo.

### US-15 — Trường thông tin động
- **AC1** — *When* chọn "Ngập lụt", *then* hiển thị "Độ sâu ước tính (cm)" và "Loại ngập".
- **AC2** — *When* chọn "Cây ngã / đổ", *then* hiển thị "Có chắn ngang đường không?".
- **AC3** — *When* chọn "Cháy rừng", *then* hiển thị "Gần khu dân cư?".
- **AC4** — *When* chọn "Kẹt xe nghiêm trọng", *then* hiển thị "Nguyên nhân" và "Độ dài kẹt xe ước tính (km)".
- **AC5** — *When* chọn loại khác, *then* hiển thị "Loại báo cáo này không yêu cầu thông tin chi tiết thêm".
- **AC6** — *When* đổi loại sự cố, *then* các trường chi tiết được đặt lại.

### US-08 / US-10 — Bản đồ & hồ sơ địa điểm
- **AC1** — Thanh chọn hiển thị 6 loại địa điểm, mỗi loại kèm % trên tổng số địa điểm.
- **AC2** — *When* chọn một loại, *then* bản đồ chỉ hiển thị marker của loại đó.
- **AC3** — *When* click marker, *then* panel hiển thị "Thông tin chung" và "Dữ liệu mới nhất"; có nút đóng.

### US-03 / US-04 — Thiệt hại thiên tai
- **AC1** — Mặc định panel chi tiết hiển thị **tổng cộng** thiệt hại của các năm có dữ liệu.
- **AC2** — *When* click cột một năm, *then* panel hiển thị chi tiết năm đó; *when* click lại cùng năm, *then* trở về tổng cộng.
- **AC3** — *When* click một hạng mục thiệt hại, *then* mở popup lịch sử của hạng mục đó.

### US-16 — Kiểm duyệt báo cáo (Planned)
- **AC1** — Cán bộ thấy danh sách báo cáo PENDING, sắp xếp mới nhất trước, lọc theo loại sự cố.
- **AC2** — Cán bộ có thể chuyển PENDING → VERIFIED hoặc PENDING → REJECTED (kèm lý do).
- **AC3** — Báo cáo đã VERIFIED/REJECTED không thể quay lại PENDING.

## 3. Ma trận truy vết (RTM)

| Mục tiêu | User story | Yêu cầu | Use case | Màn hình / API |
|----------|-----------|---------|----------|----------------|
| BO-01 | US-01, US-02 | FR-01 – FR-05 | UC-01 | Collector Data Service (khởi tạo) |
| BO-02 | US-06, US-07 | FR-11 – FR-14 | UC-03 | `/weather` · OpenWeather |
| BO-03 | US-08 – US-12 | FR-15 – FR-19 | UC-04 | `/map` · `GET /asset/map`, `/asset/{id}/profile`, `/asset/asset-list` |
| BO-04 | US-13 – US-16 | FR-20 – FR-25 | UC-05, UC-06 | `/report` · `POST /report/send-report` |
| BO-05 | US-03 – US-05 | FR-06 – FR-10 | UC-02 | `/` · `GET /static/*`, `/metric/*` |
