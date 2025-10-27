# 📊 HƯỚNG DẪN THỐNG KÊ NÂNG CAO VỚI ARCGIS REST API

## 🎯 Giới thiệu

Tính năng **Thống kê nâng cao** cho phép bạn:

- 📊 Thống kê dữ liệu trực tiếp từ ArcGIS REST API
- 📈 Hiển thị kết quả bằng bảng và biểu đồ
- 📥 Xuất kết quả ra file Excel
- 🎯 Linh hoạt cấu hình (Layer, Field, Group By)

---

## 🚀 TRUY CẬP

### Cách 1: Từ tab "Thống kê"

```
1. Mở app
2. Click tab "Thống kê" (bên phải)
3. Click button "Thống kê nâng cao"
```

### Cách 2: URL trực tiếp

```
http://localhost:8080/#/advanced-statistics
```

---

## 📝 HƯỚNG DẪN SỬ DỤNG

### Bước 1: Cấu hình

**Panel bên trái** chứa các tùy chọn:

#### 1.1. Base URL

```
Nhập URL service ArcGIS của bạn:
https://services.arcgis.com/ORG_ID/arcgis/rest/services
```

**Lưu ý:**

- Thay `ORG_ID` bằng Organization ID của bạn
- URL không có `/LayerName/FeatureServer/0` ở cuối

#### 1.2. Chọn Layer

```
Dropdown: Chọn layer cần thống kê
Ví dụ:
  - DanSo_Xa_2025
  - RanhXa_TT
  - DienTich_Huyen
```

**Lưu ý:**

- Đây là tên layer trong ArcGIS service
- Không phải tên hiển thị

#### 1.3. Chọn Field

```
Dropdown: Chọn field cần tính toán
Ví dụ:
  - Dan_So
  - Dien_Tich
  - Mat_Do
```

**Lưu ý:**

- Field phải là kiểu số (Integer, Double)
- Tên field chính xác (case-sensitive)

#### 1.4. Loại thống kê

```
Dropdown: Chọn phép toán
  - sum: Tổng
  - avg: Trung bình
  - max: Lớn nhất
  - min: Nhỏ nhất
  - count: Đếm số lượng
```

#### 1.5. Group By Field

```
TextField: Nhập field để nhóm
Ví dụ: Ten_Xa

Kết quả sẽ nhóm theo từng xã:
  - Xã A: 1200
  - Xã B: 950
  - Xã C: 1500
```

#### 1.6. Where Clause (Tùy chọn)

```
TextField: Điều kiện lọc SQL
Mặc định: 1=1 (lấy tất cả)

Ví dụ:
  - Dan_So > 1000
  - Ten_Tinh = 'Hà Nội'
  - Nam = 2025 AND Quy = 1
```

---

### Bước 2: Thực hiện thống kê

1. Click button **"Thực hiện thống kê"** (màu xanh)
2. Loading indicator hiển thị
3. Chờ API response (~2-5 giây)

---

### Bước 3: Xem kết quả

**Panel bên phải** hiển thị 2 phần:

#### 3.1. Bảng dữ liệu

```
┌─────┬──────────┬──────────┐
│ STT │   Nhóm   │  Giá trị │
├─────┼──────────┼──────────┤
│  1  │  Xã A    │  1200.00 │
│  2  │  Xã B    │   950.00 │
│  3  │  Xã C    │  1500.00 │
└─────┴──────────┴──────────┘
```

**Tính năng:**

- Sắp xếp giảm dần theo giá trị
- Dòng đan xen màu để dễ đọc
- Scroll ngang nếu nhiều cột

#### 3.2. Biểu đồ cột

```
    │
1500│     ▓▓▓
    │     ▓▓▓
1200│▓▓▓  ▓▓▓
    │▓▓▓  ▓▓▓
 950│▓▓▓  ▓▓▓  ▓▓▓
    │▓▓▓  ▓▓▓  ▓▓▓
    └───────────────
      A    C    B
```

**Tính năng:**

- Hiển thị Top 10 kết quả
- Hover để xem giá trị chính xác
- Tên nhóm xoay 90° để dễ đọc
- Màu xanh Word 2010

---

### Bước 4: Xuất Excel

1. Click button **"Xuất Excel"** (góc dưới phải, màu xanh lá)
2. File Excel tự động tải về
3. Snackbar thông báo "Xuất Excel thành công!"

**Tên file:**

```
thong_ke_[timestamp].xlsx
Ví dụ: thong_ke_1729512345678.xlsx
```

**Nội dung file Excel:**

| STT | Nhóm | Giá trị |
|-----|------|---------|
| 1 | Xã A | 1200.00 |
| 2 | Xã B | 950.00 |
| 3 | Xã C | 1500.00 |

**Định dạng:**

- Header: Background xanh (#0078D4), text trắng, bold
- Data: Text đen, số format 2 chữ số thập phân

---

## 🔧 CẤU HÌNH ARCGIS REST API

### URL Pattern

```
{baseUrl}/{layerName}/FeatureServer/0/query
```

**Ví dụ đầy đủ:**

```
https://services.arcgis.com/abc123xyz/arcgis/rest/services/DanSo_Xa_2025/FeatureServer/0/query
```

### Query Parameters

```
?where=1=1
&groupByFieldsForStatistics=Ten_Xa
&outStatistics=[{
  "statisticType":"sum",
  "onStatisticField":"Dan_So",
  "outStatisticFieldName":"sum_Dan_So"
}]
&f=json
```

**Giải thích:**

| Parameter | Mô tả | Ví dụ |
|-----------|-------|-------|
| `where` | Điều kiện lọc SQL | `1=1` (tất cả) |
| `groupByFieldsForStatistics` | Field nhóm | `Ten_Xa` |
| `outStatistics` | Định nghĩa thống kê | JSON array |
| `f` | Format response | `json` |

### Response Format

```json
{
  "features": [
    {
      "attributes": {
        "Ten_Xa": "Xã A",
        "sum_Dan_So": 1200
      }
    },
    {
      "attributes": {
        "Ten_Xa": "Xã B",
        "sum_Dan_So": 950
      }
    }
  ]
}
```

---

## 💡 VÍ DỤ CỤ THỂ

### Ví dụ 1: Tổng dân số theo xã

**Cấu hình:**

```
Layer: DanSo_Xa_2025
Field: Dan_So
Stat Type: sum
Group By: Ten_Xa
Where: 1=1
```

**API Call:**

```
GET https://.../DanSo_Xa_2025/FeatureServer/0/query
?where=1=1
&groupByFieldsForStatistics=Ten_Xa
&outStatistics=[{"statisticType":"sum","onStatisticField":"Dan_So","outStatisticFieldName":"sum_Dan_So"}]
&f=json
```

**Kết quả:**

```
Xã Tân Phú:  1200
Xã Hòa Bình: 950
Xã An Lạc:   1500
```

---

### Ví dụ 2: Diện tích trung bình theo huyện

**Cấu hình:**

```
Layer: DienTich_Huyen
Field: Dien_Tich
Stat Type: avg
Group By: Ten_Huyen
Where: Tinh = 'Hà Nội'
```

**Kết quả:**

```
Huyện A: 125.5 km²
Huyện B: 98.3 km²
Huyện C: 156.8 km²
```

---

### Ví dụ 3: Đếm số trường học theo quận

**Cấu hình:**

```
Layer: TruongHoc_2025
Field: OBJECTID
Stat Type: count
Group By: Ten_Quan
Where: Loai = 'Trung học'
```

**Kết quả:**

```
Quận 1: 15 trường
Quận 2: 12 trường
Quận 3: 18 trường
```

---

## 🔍 TROUBLESHOOTING

### Lỗi: "Không có dữ liệu"

**Nguyên nhân:**

- Where clause không match record nào
- Group By field sai
- Layer không có dữ liệu

**Giải pháp:**

1. Kiểm tra Where clause
2. Thử `where=1=1` để lấy tất cả
3. Kiểm tra tên field chính xác
4. Test API trong browser trước

---

### Lỗi API: 400/500

**Nguyên nhân:**

- URL sai
- Layer name sai
- Field name không tồn tại
- Syntax SQL sai

**Giải pháp:**

1. Test URL trong browser:

   ```
   https://.../LayerName/FeatureServer/0?f=json
   ```

2. Xem list fields available
3. Kiểm tra capabilities (có support statistics không)

---

### Lỗi CORS

**Nguyên nhân:**

- ArcGIS service không cho phép domain của bạn

**Giải pháp:**

1. Thêm domain vào whitelist trong ArcGIS Online
2. Hoặc dùng proxy
3. Hoặc enable CORS trên server

---

### Excel không download (Web)

**Nguyên nhân:**

- Browser block download
- Popup blocker

**Giải pháp:**

1. Allow downloads trong browser settings
2. Disable popup blocker
3. Thử browser khác (Chrome khuyến nghị)

---

## 🎨 TÙY CHỈNH

### Thêm Layer mới

**File:** `statistics_page.dart`

**Tìm dòng:**

```dart
final List<String> _layers = [
  'DanSo_Xa_2025',
  'RanhXa_TT',
  'DienTich_Huyen',
];
```

**Thêm:**

```dart
final List<String> _layers = [
  'DanSo_Xa_2025',
  'RanhXa_TT',
  'DienTich_Huyen',
  'TruongHoc_2025',  // THÊM
  'TramYTe_2025',    // THÊM
];
```

---

### Thêm Field mới

**Tìm dòng:**

```dart
final List<String> _fields = [
  'Dan_So',
  'Dien_Tich',
  'Mat_Do',
];
```

**Thêm:**

```dart
final List<String> _fields = [
  'Dan_So',
  'Dien_Tich',
  'Mat_Do',
  'So_Hoc_Sinh',  // THÊM
  'Kinh_Phi',     // THÊM
];
```

---

### Thay đổi màu biểu đồ

**Tìm dòng:**

```dart
BarChartRodData(
  toY: entry.value.value,
  color: const Color(0xFF0078D4),  // ← ĐỔI MÀU Ở ĐÂY
  width: 20,
  ...
)
```

**Màu gợi ý:**

- Xanh: `0xFF0078D4` (Word 2010 blue)
- Xanh lá: `0xFF107C10` (Excel green)
- Cam: `0xFFFF8C00`
- Tím: `0xFF5C2D91`

---

## 📚 API REFERENCE

### fetchStatistics()

```dart
Future<void> fetchStatistics({
  required String baseUrl,
  String? whereClause,
});
```

**Parameters:**

- `baseUrl`: URL gốc của ArcGIS service
- `whereClause`: SQL where clause (optional, default: "1=1")

**State required:**

- `selectedLayer`: Tên layer
- `selectedField`: Tên field cần thống kê
- `selectedStatType`: Loại thống kê (sum/avg/max/min/count)
- `groupByField`: Field để nhóm kết quả

**Returns:**

- Cập nhật state với `List<StatisticResult>`

**Throws:**

- Emit error nếu có lỗi network hoặc API

---

### StatisticResult Model

```dart
class StatisticResult {
  final String group;   // Tên nhóm (từ groupBy field)
  final double value;   // Giá trị thống kê
  
  StatisticResult({
    required this.group,
    required this.value,
  });
}
```

---

## 🔬 API TESTING

### Test trong Browser

```
1. Copy URL từ console log
2. Paste vào browser
3. Xem JSON response
4. Kiểm tra cấu trúc dữ liệu
```

**URL mẫu:**

```
https://services.arcgis.com/ORG_ID/arcgis/rest/services/DanSo_Xa_2025/FeatureServer/0/query?where=1=1&groupByFieldsForStatistics=Ten_Xa&outStatistics=[{"statisticType":"sum","onStatisticField":"Dan_So","outStatisticFieldName":"sum_Dan_So"}]&f=json
```

**Response mẫu:**

```json
{
  "displayFieldName": "",
  "fieldAliases": {
    "Ten_Xa": "Tên xã",
    "sum_Dan_So": "Tổng dân số"
  },
  "fields": [...],
  "features": [
    {
      "attributes": {
        "Ten_Xa": "Xã Tân Phú",
        "sum_Dan_So": 1200
      }
    }
  ]
}
```

---

## 📥 XUẤT EXCEL

### Cấu trúc File

**Sheet name:** "Thống kê"

**Columns:**

1. STT - Số thứ tự
2. Nhóm - Tên nhóm (từ groupBy field)
3. Giá trị - Kết quả thống kê

**Styling:**

- Header row: Background xanh #0078D4, text trắng, bold
- Data rows: Text đen, số có 2 chữ số thập phân

**File name:**

```
thong_ke_[timestamp].xlsx
```

### Download trên Web

App sử dụng `html.AnchorElement` để trigger download:

```dart
final blob = html.Blob([bytes], 'application/vnd.openxmlformats-officedocument.spreadsheetml.sheet');
final url = html.Url.createObjectUrlFromBlob(blob);
final anchor = html.AnchorElement(href: url)
  ..setAttribute('download', 'thong_ke_${timestamp}.xlsx')
  ..click();
```

---

## 💻 CODE STRUCTURE

### Files

```
statistics_page/
├── statistics_page.dart           ← UI và layout
├── models/
│   └── statistic_result.dart      ← Model class
└── cubit/
    ├── advanced_statistics_cubit.dart  ← Business logic
    └── advanced_statistics_state.dart  ← State management
```

### Dependencies

```yaml
dependencies:
  fl_chart: ^0.69.0       # Bar chart visualization
  excel: ^4.0.6           # Excel export
  http: ^1.2.2            # HTTP requests
  path_provider: ^2.1.5   # File paths (mobile/desktop)
```

---

## 🎯 USE CASES

### 1. Thống kê dân số

**Mục đích:** Tính tổng dân số từng xã

```
Layer: DanSo_Xa_2025
Field: Dan_So
Stat: sum
Group By: Ten_Xa
Where: 1=1
```

**Kết quả:** Biết xã nào đông dân nhất

---

### 2. Diện tích trung bình

**Mục đích:** Tính diện tích trung bình các khu vực

```
Layer: KhuVuc_QuyHoach
Field: Dien_Tich
Stat: avg
Group By: Loai_Khu_Vuc
Where: 1=1
```

**Kết quả:** So sánh diện tích các loại khu vực

---

### 3. Đếm số công trình

**Mục đích:** Đếm số công trình theo từng quận

```
Layer: CongTrinh_CongCong
Field: OBJECTID
Stat: count
Group By: Ten_Quan
Where: Trang_Thai = 'Hoạt động'
```

**Kết quả:** Biết quận nào có nhiều công trình nhất

---

### 4. Tìm giá trị cao nhất

**Mục đích:** Tìm chiều cao cao nhất của các tòa nhà theo khu vực

```
Layer: ToaNha_ThuDo
Field: Chieu_Cao
Stat: max
Group By: Khu_Vuc
Where: 1=1
```

**Kết quả:** Biết tòa nhà cao nhất mỗi khu vực

---

## 🔐 BẢO MẬT

### Token Authentication

Nếu ArcGIS service yêu cầu token:

**Cách 1: Thêm token vào URL**

```dart
final queryParams = {
  'where': whereClause ?? '1=1',
  'groupByFieldsForStatistics': state.groupByField,
  'outStatistics': jsonEncode(outStatistics),
  'token': 'YOUR_TOKEN_HERE',  // THÊM
  'f': 'json',
};
```

**Cách 2: Header Authorization**

```dart
final response = await http.get(
  uri,
  headers: {
    'Authorization': 'Bearer YOUR_TOKEN_HERE',
  },
);
```

### Public Service

Nếu service là public, không cần token.

---

## 🚀 ADVANCED FEATURES (Tương lai)

### 1. Multiple Statistics

Thống kê nhiều field cùng lúc:

```json
"outStatistics": [
  {"statisticType":"sum","onStatisticField":"Dan_So","outStatisticFieldName":"sum_Dan_So"},
  {"statisticType":"avg","onStatisticField":"Dien_Tich","outStatisticFieldName":"avg_Dien_Tich"}
]
```

### 2. Nested GroupBy

Group theo nhiều level:

```
groupByFieldsForStatistics=Ten_Tinh,Ten_Xa
```

### 3. Chart Types

Thêm Pie chart, Line chart:

```dart
// Toggle chart type
enum ChartType { bar, pie, line }
```

### 4. Save Configuration

Lưu cấu hình đã dùng:

```dart
// Save to SharedPreferences
await prefs.setString('last_config', jsonEncode(config));
```

---

## 📞 HỖ TRỢ

### Tài liệu ArcGIS

- **Query API**: <https://developers.arcgis.com/rest/services-reference/query-feature-service-layer-.htm>
- **Statistics**: <https://developers.arcgis.com/rest/services-reference/query-feature-service-layer-.htm#ESRI_SECTION1_40EFC82427794E7DB4DDCAC0968FDAC7>

### Community

- **GIS StackExchange**: <https://gis.stackexchange.com>
- **Esri Community**: <https://community.esri.com>

### Contact

- Email: [your-support-email]
- Hotline: [your-phone]

---

## ✅ CHECKLIST

```
□ Đã cài đặt dependencies (fl_chart, excel, http)
□ Đã có Base URL của ArcGIS service
□ Đã biết tên Layer cần thống kê
□ Đã biết tên các Fields trong layer
□ Đã test API trong browser
□ Đã hiểu cách cấu hình
□ Đã test xuất Excel
□ Đã đọc ví dụ cụ thể
```

---

**Cập nhật:** 21/10/2025  
**Phiên bản:** 1.1.0  
**Tác giả:** Ho Doan
