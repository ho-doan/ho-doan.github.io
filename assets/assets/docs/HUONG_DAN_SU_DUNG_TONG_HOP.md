# 📚 HƯỚNG DẪN SỬ DỤNG TỔNG HỢP - ỨNG DỤNG WEB GIS CHO CÁC XÃ

## 🎯 MỤC ĐÍCH TÀI LIỆU

Tài liệu này được tạo ra để hướng dẫn **cán bộ xã** (người chưa có nhiều kiến thức kỹ thuật) có thể:

1. ✅ Hiểu cách hoạt động của ứng dụng
2. ✅ Tự tạo và cập nhật bản đồ cho xã mình
3. ✅ Chỉnh sửa dữ liệu khi cần
4. ✅ Triển khai cho các xã khác

---

## 📋 DANH MỤC TÀI LIỆU

### 📖 1. Hướng dẫn Code và Menu

**File**: `HUONG_DAN_CODE_VA_MENU.md`

**Nội dung:**

- Giới thiệu nền tảng công nghệ
- Cấu trúc dự án Flutter
- Cách chạy ứng dụng
- **Hướng dẫn chỉnh sửa menu** (Thêm/bớt/đổi tên tab)
- Tùy chỉnh giao diện

**Dành cho:**

- Lập trình viên
- Người muốn tùy chỉnh giao diện
- Người cần thêm tính năng mới

**Ví dụ thực tế:**

```txt
Bạn muốn thêm tab "Báo cáo" mới?
→ Xem phần "5. THÊM TAB MENU MỚI"
→ Follow từng bước 1 → 6
→ Chạy lại app
```

---

### 🗺️ 2. Hướng dẫn Chuyển Layer

**File**: `HUONG_DAN_CHUYEN_LAYER_ARCGIS.md`

**Nội dung:**

- Quy trình chuyển dữ liệu: Excel/CAD → ArcGIS Pro → ArcGIS Online → Web
- Import dữ liệu từ Excel
- Thiết lập symbology (màu sắc, biểu tượng)
- Cấu hình pop-up
- Publish lên ArcGIS Online
- **Overwrite (cập nhật) layer khi có thay đổi**
- Tích hợp vào Web App

**Dành cho:**

- Cán bộ xã quản lý dữ liệu
- Người cập nhật bản đồ định kỳ
- Người mới bắt đầu với ArcGIS

**Ví dụ thực tế:**

```txt
Xã có danh sách 20 trường học trong Excel
→ Muốn hiển thị lên bản đồ web

Làm theo:
1. Phần "2. CHUẨN BỊ DỮ LIỆU" → Import Excel
2. Phần "3. THIẾT LẬP SYMBOLOGY" → Chọn biểu tượng
3. Phần "4. PUBLISH" → Đưa lên web
4. Phần "5. TÍCH HỢP VÀO APP" → Thêm vào code
```

---

### ✏️ 3. Hướng dẫn Chỉnh sửa Layer

**File**: `HUONG_DAN_CHINH_SUA_LAYER.md`

**Nội dung:**

- Chỉnh sửa trong ArcGIS Pro
- Chỉnh sửa trên ArcGIS Online
- Quản lý Attributes (thông tin)
- Chỉnh sửa Geometry (vị trí, hình dạng)
- Thêm/xóa features
- Best practices

**Dành cho:**

- Người cập nhật dữ liệu thường xuyên
- Người sửa lỗi dữ liệu
- Người thêm/bớt điểm trên bản đồ

**Ví dụ thực tế:**

```txt
Trường học A đã chuyển địa điểm mới
→ Cần cập nhật vị trí trên bản đồ

Cách 1: Sửa trong ArcGIS Pro
→ Xem phần "2.B. CHỈNH SỬA GEOMETRY"

Cách 2: Sửa trực tiếp trên ArcGIS Online
→ Xem phần "3.B. CHỈNH SỬA FEATURES"
```

---

## 🔄 QUY TRÌNH TỔNG QUÁT

### Kịch bản 1: Tạo bản đồ mới cho xã

```txt
┌─────────────────────────────────────────┐
│ BƯỚC 1: Chuẩn bị dữ liệu Excel          │
├─────────────────────────────────────────┤
│ Tạo file Excel với cấu trúc:            │
│ - ID                                    │
│ - TenDiem                               │
│ - KinhDo (Longitude)                    │
│ - ViDo (Latitude)                       │
│ - LoaiDiem (Phân loại)                  │
│ - Các thông tin khác...                 │
│                                         │
│ 📄 Xem: HUONG_DAN_CHUYEN_LAYER.md      │
│    → Phần "2. CHUẨN BỊ DỮ LIỆU"        │
└─────────────────────────────────────────┘
              ⬇
┌─────────────────────────────────────────┐
│ BƯỚC 2: Import vào ArcGIS Pro           │
├─────────────────────────────────────────┤
│ 1. Mở ArcGIS Pro                        │
│ 2. Add Data → Chọn Excel                │
│ 3. XY Table To Point                    │
│ 4. Thiết lập symbology                  │
│ 5. Cấu hình pop-up                      │
│                                         │
│ 📄 Xem: HUONG_DAN_CHUYEN_LAYER.md      │
│    → Phần "2. CHUẨN BỊ TRONG PRO"       │
└─────────────────────────────────────────┘
              ⬇
┌─────────────────────────────────────────┐
│ BƯỚC 3: Publish lên ArcGIS Online       │
├─────────────────────────────────────────┤
│ 1. Sign in ArcGIS Online                │
│ 2. Share As Web Layer                   │
│ 3. Thiết lập tên, mô tả, tags           │
│ 4. Chọn "Public" để công khai           │
│ 5. Publish                              │
│ 6. Copy Service URL                     │
│                                         │
│ 📄 Xem: HUONG_DAN_CHUYEN_LAYER.md      │
│    → Phần "3. PUBLISH"                  │
└─────────────────────────────────────────┘
              ⬇
┌─────────────────────────────────────────┐
│ BƯỚC 4: Tích hợp vào Web App            │
├─────────────────────────────────────────┤
│ 1. Thêm config layer mới trong code    │
│ 2. Load layer vào map                   │
│ 3. Test trên web                        │
│ 4. Build và deploy                      │
│                                         │
│ 📄 Xem: HUONG_DAN_CODE_VA_MENU.md      │
│    → Phần "5. THÊM TAB MENU MỚI"        │
└─────────────────────────────────────────┘
```

---

### Kịch bản 2: Cập nhật dữ liệu định kỳ

```txt
┌─────────────────────────────────────────┐
│ TÌNH HUỐNG: Hàng tháng cập nhật dữ liệu │
│ (VD: Thêm 5 trường học mới)             │
└─────────────────────────────────────────┘
              ⬇
┌─────────────────────────────────────────┐
│ CÁCH 1: Sửa trong ArcGIS Pro (Khuyến nghị)│
├─────────────────────────────────────────┤
│ 1. Mở project ArcGIS Pro                │
│ 2. Thêm 5 điểm mới (Create Features)    │
│ 3. Nhập thông tin vào Attribute Table   │
│ 4. Save                                 │
│ 5. Overwrite layer lên Online          │
│                                         │
│ 📄 Xem: HUONG_DAN_CHINH_SUA_LAYER.md   │
│    → Phần "6. THÊM/XÓA FEATURES"        │
│ 📄 Xem: HUONG_DAN_CHUYEN_LAYER.md      │
│    → Phần "4. OVERWRITE"                │
└─────────────────────────────────────────┘
              ⬇
┌─────────────────────────────────────────┐
│ CÁCH 2: Sửa trực tiếp trên Online       │
│ (Nếu không có ArcGIS Pro)               │
├─────────────────────────────────────────┤
│ 1. Đăng nhập ArcGIS Online              │
│ 2. Mở Map Viewer                        │
│ 3. Edit mode → Add features             │
│ 4. Click vào map → Nhập thông tin       │
│ 5. Save                                 │
│                                         │
│ 📄 Xem: HUONG_DAN_CHINH_SUA_LAYER.md   │
│    → Phần "3. CHỈNH SỬA TRÊN ONLINE"    │
└─────────────────────────────────────────┘
              ⬇
┌─────────────────────────────────────────┐
│ KẾT QUẢ                                 │
├─────────────────────────────────────────┤
│ ✅ Dữ liệu đã cập nhật                  │
│ ✅ Web app tự động hiển thị mới         │
│ ✅ Không cần build lại app              │
└─────────────────────────────────────────┘
```

---

### Kịch bản 3: Thêm tính năng mới (Tab menu)

```txt
┌─────────────────────────────────────────┐
│ TÌNH HUỐNG: Muốn thêm tab "Báo cáo"     │
└─────────────────────────────────────────┘
              ⬇
┌─────────────────────────────────────────┐
│ BƯỚC 1: Thêm tên tab                    │
├─────────────────────────────────────────┤
│ File: home_page.dart                    │
│ Dòng 28:                                │
│                                         │
│ const _actions = [                      │
│   'Lớp bản đồ',                         │
│   'Tìm kiếm',                           │
│   ...                                   │
│   'Báo cáo',  // ← THÊM                 │
│ ];                                      │
│                                         │
│ 📄 Xem: HUONG_DAN_CODE_VA_MENU.md      │
│    → Phần "5. THÊM TAB MENU MỚI"        │
│    → Bước 1                             │
└─────────────────────────────────────────┘
              ⬇
┌─────────────────────────────────────────┐
│ BƯỚC 2: Tạo widget cho tab              │
├─────────────────────────────────────────┤
│ Tạo file:                               │
│ widgets/tabs/report/report.dart         │
│                                         │
│ Copy template từ GuideTab hoặc         │
│ StatisticsTab                           │
│                                         │
│ 📄 Xem: HUONG_DAN_CODE_VA_MENU.md      │
│    → Phần "5. THÊM TAB MENU MỚI"        │
│    → Bước 3                             │
└─────────────────────────────────────────┘
              ⬇
┌─────────────────────────────────────────┐
│ BƯỚC 3: Import và thêm vào TabBarView   │
├─────────────────────────────────────────┤
│ 1. Import widget mới                    │
│ 2. Thêm vào children của TabBarView     │
│ 3. Tăng itemCount                       │
│                                         │
│ 📄 Xem: HUONG_DAN_CODE_VA_MENU.md      │
│    → Phần "5. THÊM TAB MENU MỚI"        │
│    → Bước 4-5                           │
└─────────────────────────────────────────┘
              ⬇
┌─────────────────────────────────────────┐
│ BƯỚC 4: Chạy lại app                    │
├─────────────────────────────────────────┤
│ flutter run -d chrome                   │
│                                         │
│ Kiểm tra tab mới đã xuất hiện           │
└─────────────────────────────────────────┘
```

---

## 🛠️ CÔNG CỤ CẦN THIẾT

### Phần mềm bắt buộc

| Phần mềm | Mục đích | Download |
|----------|----------|----------|
| **ArcGIS Pro** | Xử lý dữ liệu, tạo layer | <https://pro.arcgis.com> |
| **Microsoft Excel** | Quản lý dữ liệu | Office 365 |
| **Web Browser** (Chrome) | Truy cập ArcGIS Online, test app | <https://google.com/chrome> |

### Phần mềm tùy chọn (cho lập trình viên)

| Phần mềm | Mục đích | Download |
|----------|----------|----------|
| **Flutter SDK** | Build và chạy app | <https://flutter.dev> |
| **VS Code** | Chỉnh sửa code | <https://code.visualstudio.com> |
| **Git** | Version control | <https://git-scm.com> |

### Tài khoản cần thiết

- ✅ **ArcGIS Online Account** (Có quyền publish)
  - Đăng ký: <https://www.arcgis.com>
  - Cần license Creator hoặc GIS Professional

---

## 📊 CẤU TRÚC DỮ LIỆU EXCEL MẪU

### Template chuẩn

| ID | TenDiem | MoTa | LoaiDiem | KinhDo | ViDo | TrangThai | NamThanhLap |
|----|---------|------|----------|---------|------|-----------|-------------|
| 1 | Trường THCS Tân Phú | Trường trung học cơ sở | Giáo dục | 105.8234 | 21.0245 | Hoạt động | 1995 |
| 2 | Trạm y tế xã | Trạm y tế cấp xã | Y tế | 105.8456 | 21.0367 | Hoạt động | 2005 |
| 3 | UBND xã Tân Phú | Ủy ban nhân dân | Hành chính | 105.8512 | 21.0401 | Hoạt động | 1990 |

### Quy tắc đặt tên cột

✅ **Đúng:**

- TenDiem, LoaiDiem, KinhDo, ViDo
- ID, MoTa, TrangThai, NamThanhLap

❌ **Sai:**

- Tên điểm (có khoảng trắng)
- Loại_điểm (có dấu)
- Kinh độ (có khoảng trắng + dấu)
- 1_ID (bắt đầu bằng số)

### Lưu ý về tọa độ

```txt
Hệ tọa độ: WGS 1984 (EPSG:4326)

KinhDo (Longitude): 102.0 → 110.0 (Việt Nam)
ViDo (Latitude): 8.0 → 24.0 (Việt Nam)

Ví dụ Hà Nội: 
- KinhDo: 105.8342
- ViDo: 21.0278

Cách lấy tọa độ:
1. Google Maps → Chuột phải vào điểm
2. Click vào tọa độ → Copy
3. Paste vào Excel
```

---

## 🚀 QUY TRÌNH TRIỂN KHAI CHO XÃ MỚI

### Checklist triển khai

```txt
□ 1. Thu thập dữ liệu
   □ Danh sách các điểm quan trọng
   □ Tọa độ (GPS hoặc Google Maps)
   □ Thông tin chi tiết từng điểm
   □ Hình ảnh (nếu có)

□ 2. Chuẩn bị file Excel
   □ Tạo file theo template
   □ Nhập dữ liệu đầy đủ
   □ Kiểm tra tọa độ
   □ Phân loại đúng

□ 3. Xử lý trong ArcGIS Pro
   □ Import Excel
   □ Tạo feature layer
   □ Thiết lập symbology
   □ Cấu hình pop-up
   □ Kiểm tra dữ liệu

□ 4. Publish lên ArcGIS Online
   □ Đăng nhập account
   □ Share as Web Layer
   □ Đặt tên, mô tả, tags
   □ Public layer
   □ Copy URL

□ 5. Tích hợp vào Web App
   □ Thêm layer config
   □ Update code
   □ Test local
   □ Build production

□ 6. Deploy
   □ Build web
   □ Upload lên hosting
   □ Test trên production
   □ Gửi link cho xã

□ 7. Hướng dẫn sử dụng
   □ Tạo tài liệu hướng dẫn
   □ Đào tạo cán bộ xã
   □ Hỗ trợ khi cần
```

### Thời gian ước tính

- **Lần đầu** (Học + Làm): 1-2 tuần
- **Lần sau** (Đã quen): 2-3 ngày
- **Chỉ cập nhật dữ liệu**: 1-2 giờ

---

## ❓ CÂU HỎI THƯỜNG GẶP (FAQ)

### 1. Tôi chưa biết lập trình, có làm được không?

**Trả lời:**

- ✅ **CÓ** - Nếu chỉ cập nhật dữ liệu
  - Dùng ArcGIS Pro hoặc ArcGIS Online
  - Xem: `HUONG_DAN_CHINH_SUA_LAYER.md`

- ⚠️ **CẦN HỌC THÊM** - Nếu muốn thay đổi giao diện
  - Cần biết Flutter cơ bản
  - Xem: `HUONG_DAN_CODE_VA_MENU.md`

### 2. Làm sao để thêm 1 điểm mới vào bản đồ?

**Cách 1: Trong ArcGIS Pro**

```txt
1. Mở layer cần thêm
2. Tab "Edit" → "Create"
3. Click vào map → Đặt điểm
4. Nhập thông tin
5. Save
6. Overwrite layer lên Online
```

**Cách 2: Trên ArcGIS Online**

```txt
1. Mở Map Viewer
2. Bật Edit mode
3. Click "New feature"
4. Click vào map
5. Nhập thông tin → Save
```

📄 **Xem chi tiết:** `HUONG_DAN_CHINH_SUA_LAYER.md` → Phần 6

### 3. Dữ liệu đã cập nhật nhưng web không thay đổi?

**Nguyên nhân:**

- Cache browser chưa refresh
- Layer chưa overwrite đúng

**Giải pháp:**

```txt
1. Hard refresh browser: Ctrl + Shift + R
2. Xóa cache browser
3. Kiểm tra layer trên ArcGIS Online có mới không
4. Nếu vẫn không đổi → Kiểm tra URL layer trong code
```

### 4. Làm sao biết tọa độ một điểm?

**Cách 1: Google Maps**

```txt
1. Mở Google Maps
2. Chuột phải vào điểm cần lấy tọa độ
3. Click vào số tọa độ hiện ra
4. Copy: "21.0278, 105.8342"
   → ViDo: 21.0278
   → KinhDo: 105.8342
```

**Cách 2: Thiết bị GPS**

```txt
1. Đứng tại điểm cần đo
2. Mở app GPS (Google Maps trên điện thoại)
3. Xem tọa độ hiện tại
4. Ghi lại
```

**Cách 3: Trong ArcGIS Pro**

```txt
1. Mở map
2. Di chuột đến điểm cần biết tọa độ
3. Xem góc dưới bên phải (status bar)
4. Hiển thị tọa độ real-time
```

### 5. Tôi muốn thay đổi màu sắc của layer?

**Trong ArcGIS Pro:**

```txt
1. Chuột phải layer → Symbology
2. Chọn màu mới
3. Apply
4. Overwrite layer lên Online
```

**Trên ArcGIS Online:**

```
1. Content → Chọn layer
2. Tab Visualization
3. Change Style
4. Chọn màu mới → Done
```

📄 **Xem chi tiết:** `HUONG_DAN_CHUYEN_LAYER.md` → Phần 2.3

### 6. Layer bị lỗi không hiển thị?

**Checklist khắc phục:**

```txt
□ URL layer có đúng không?
   → Kiểm tra trong code

□ Layer có public không?
   → ArcGIS Online → Settings → Share with Everyone

□ Geometry có hợp lệ không?
   → Check Geometry trong ArcGIS Pro

□ Coordinate system đúng không?
   → Phải là WGS 1984 (EPSG:4326)

□ Có data không?
   → Mở layer trên ArcGIS Online kiểm tra
```

---

## 📞 HỖ TRỢ VÀ LIÊN HỆ

### Khi gặp vấn đề

1. **Kiểm tra tài liệu**
   - Đọc kỹ các file hướng dẫn
   - Tìm phần FAQ

2. **Google tìm kiếm**
   - "ArcGIS Pro [vấn đề của bạn]"
   - "Flutter [lỗi bạn gặp]"

3. **Diễn đàn**
   - Esri Community: <https://community.esri.com>
   - Stack Overflow: <https://stackoverflow.com>

4. **Liên hệ hỗ trợ**
   - Email: [your-support-email]
   - Hotline: [your-phone]

---

## 📚 TÀI LIỆU THAM KHẢO

### Tài liệu chính thức

- **ArcGIS Pro**: <https://pro.arcgis.com/en/pro-app/latest/help/>
- **ArcGIS Online**: <https://doc.arcgis.com/en/arcgis-online/>
- **Flutter**: <https://flutter.dev/docs>
- **ArcGIS SDK for JavaScript**: <https://developers.arcgis.com/javascript/>

### Video hướng dẫn

- **Esri Training**: <https://www.esri.com/training/>
- **YouTube**: "ArcGIS Pro Tutorial for Beginners"
- **YouTube**: "Flutter Web Tutorial"

### Khóa học online

- **Esri Academy**: <https://www.esri.com/training/catalog/search/>
- **Udemy**: "ArcGIS Pro Complete Course"
- **Coursera**: "GIS Specialization"

---

## 🔄 CẬP NHẬT VÀ BẢO TRÌ

### Lịch trình bảo trì

| Tần suất | Công việc | Người thực hiện |
|----------|-----------|-----------------|
| **Hàng ngày** | Kiểm tra app hoạt động | Admin xã |
| **Hàng tuần** | Cập nhật dữ liệu nếu có | Cán bộ xã |
| **Hàng tháng** | Backup dữ liệu | Admin |
| **Hàng quý** | Review và tối ưu | Lập trình viên |
| **Hàng năm** | Nâng cấp tính năng mới | Lập trình viên |

### Version control

- Lưu lại mỗi lần thay đổi quan trọng
- Ghi chú: Ngày, người sửa, nội dung sửa
- Backup trước khi update lớn

---

## ✅ CHECKLIST HOÀN THÀNH

Sau khi hoàn thành, bạn sẽ có:

```txt
✅ Ứng dụng web GIS hoạt động
✅ Dữ liệu bản đồ đầy đủ cho xã
✅ Hiểu cách cập nhật dữ liệu
✅ Có thể triển khai cho xã khác
✅ Tự tin bảo trì và nâng cấp
```

---

**🎉 Chúc bạn triển khai thành công!**

**Ngày tạo**: 21/10/2025  
**Phiên bản**: 1.0  
**Tác giả**: Ho Doan  
**Liên hệ**: [your-contact]

---

## 📎 PHỤ LỤC

### A. Danh sách file trong dự án

```txt
arcgis_web/
├── HUONG_DAN_CODE_VA_MENU.md           ← Hướng dẫn code
├── HUONG_DAN_CHUYEN_LAYER_ARCGIS.md    ← Hướng dẫn chuyển layer
├── HUONG_DAN_CHINH_SUA_LAYER.md        ← Hướng dẫn chỉnh sửa
├── HUONG_DAN_SU_DUNG_TONG_HOP.md       ← File này
├── README.MD                            ← Tổng quan dự án (gốc)
└── example/
    ├── lib/
    │   └── src/
    │       └── features/
    │           └── home/
    │               ├── home_page.dart   ← File chính
    │               └── widgets/
    │                   └── tabs/        ← Các tab menu
    └── assets/                          ← Hình ảnh, icons
```

### B. Shortcuts hữu ích

**ArcGIS Pro:**

- `Ctrl + T`: Mở Attribute Table
- `E`: Bật Edit mode
- `Ctrl + S`: Save edits
- `Ctrl + Z`: Undo
- `F5`: Refresh map

**VS Code:**

- `Ctrl + P`: Quick file open
- `Ctrl + Shift + F`: Tìm trong toàn bộ dự án
- `Ctrl + /`: Comment/Uncomment
- `F5`: Run/Debug

**Chrome:**

- `Ctrl + Shift + R`: Hard refresh
- `F12`: Developer tools
- `Ctrl + Shift + I`: Inspect element

---

**HẾT**
