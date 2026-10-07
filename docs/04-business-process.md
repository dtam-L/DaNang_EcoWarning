# Quy trình nghiệp vụ — DaNang EcoWarning

## 1. Quy trình báo cáo sự cố

### 1.1 As-is (khi chưa có hệ thống)
- Người dân báo sự cố qua điện thoại, mạng xã hội hoặc báo trực tiếp cho chính quyền địa phương.
- Thông tin **không có cấu trúc** (thiếu tọa độ, thiếu ảnh, mô tả tự do), khó tổng hợp và dễ trùng lặp.
- Cơ quan quản lý mất thời gian xác minh vị trí và mức độ sự cố.

### 1.2 To-be (với DaNang EcoWarning)

```mermaid
flowchart TD
    subgraph ND[Người dân]
        A([Phát hiện sự cố]) --> B[Mở trang Báo cáo]
        B --> C[Chọn loại sự cố]
        C --> D[Chọn vị trí trên bản đồ<br/>nhập mô tả, chi tiết, ảnh]
        D --> E[Bấm Gửi]
    end

    subgraph HT[Hệ thống]
        E --> F{Dữ liệu hợp lệ?<br/>BR-01 → BR-06}
        F -- Không --> G[Báo lỗi cụ thể]
        G --> D
        F -- Có --> H[Lưu ảnh lên Cloudinary]
        H --> I[Tạo báo cáo<br/>trạng thái PENDING]
        I --> J[Thông báo gửi thành công]
    end

    subgraph CB[Cán bộ quản lý — Planned]
        I --> K[Xem báo cáo chờ duyệt]
        K --> L{Thông tin chính xác?}
        L -- Có --> M[VERIFIED<br/>hiển thị trên bản đồ cộng đồng]
        L -- Không --> N[REJECTED<br/>ghi lý do]
    end
```

**Giá trị mang lại:** báo cáo luôn có tọa độ trong Đà Nẵng, loại sự cố chuẩn hóa, có ảnh hiện trường và trạng thái xử lý rõ ràng. Nhờ đó cơ quan quản lý xác minh và tổng hợp nhanh hơn.

## 2. Vòng đời trạng thái báo cáo

```mermaid
stateDiagram-v2
    [*] --> PENDING: Người dân gửi báo cáo hợp lệ
    PENDING --> VERIFIED: Cán bộ xác minh (Planned)
    PENDING --> REJECTED: Cán bộ từ chối + lý do (Planned)
    VERIFIED --> [*]
    REJECTED --> [*]
```

| Trạng thái | Ý nghĩa | Ai được chuyển |
|-----------|---------|---------------|
| PENDING | Mới gửi, chờ xác minh | Hệ thống (tự động khi tạo) |
| VERIFIED | Đã xác minh là chính xác | Cán bộ quản lý |
| REJECTED | Sai / trùng / spam | Cán bộ quản lý |

## 3. Quy trình nạp dữ liệu mở

```mermaid
flowchart TD
    S([Khởi động hệ thống]) --> C{CSDL đã có dữ liệu?}
    C -- Có --> X([Bỏ qua — tránh trùng lặp])
    C -- Chưa --> R[Tạo mã lần chạy runId]
    R --> A[Nạp 9 file danh mục địa điểm<br/>hồ, sông, sạt lở, trạm cảnh báo, nhà trú bão]
    A --> O[Nạp các file số liệu<br/>thời tiết, mực nước, môi trường, thiệt hại, nông nghiệp]
    O --> L[Ghi nhật ký từng file:<br/>trạng thái, số bản ghi xử lý / thêm mới, lỗi]
    L --> E([Dữ liệu sẵn sàng cho Dashboard & Bản đồ])
    A -. bản ghi lỗi .-> W[Ghi log, bỏ qua bản ghi]
    O -. bản ghi lỗi .-> W
```

## 4. Hành trình người dùng (User Journey) — mùa mưa bão

| Bước | Người dân làm gì | Màn hình hỗ trợ |
|------|------------------|-----------------|
| 1 | Kiểm tra dự báo mưa, gió 5 ngày tới | Thời tiết |
| 2 | Tìm nhà trú bão gần nhà, xem khu vực từng sạt lở | Bản đồ rủi ro |
| 3 | Phát hiện đường ngập / cây đổ → báo cáo kèm ảnh | Báo cáo sự cố |
| 4 | *(Planned)* Xem các sự cố đã xác minh xung quanh | Bản đồ rủi ro |
