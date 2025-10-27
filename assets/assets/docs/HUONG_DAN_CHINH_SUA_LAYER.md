<!-- spell-checker: disable -->
# HƯỚNG DẪN CHỈNH SỬA CÁC LỚP DỮ LIỆU

## 📋 MỤC LỤC

1. [Tổng quan về Layer](#1-tổng-quan-về-layer)
2. [Chỉnh sửa Layer trong ArcGIS Pro](#2-chỉnh-sửa-layer-trong-arcgis-pro)
3. [Chỉnh sửa Layer trên ArcGIS Online](#3-chỉnh-sửa-layer-trên-arcgis-online)
4. [Quản lý Attributes (Thuộc tính)](#4-quản-lý-attributes-thuộc-tính)
5. [Chỉnh sửa Geometry (Hình học)](#5-chỉnh-sửa-geometry-hình-học)
6. [Thêm/Xóa Features](#6-thêm-xóa-features)
7. [Best Practices](#7-best-practices)

---

## 1. TỔNG QUAN VỀ LAYER

### 📚 Khái niệm cơ bản

**Layer là gì?**

- Layer (lớp) là một tập hợp dữ liệu không gian địa lý
- Mỗi layer chứa nhiều features (đối tượng)
- Mỗi feature có:
  - **Geometry**: Hình dạng (điểm, đường, vùng)
  - **Attributes**: Thông tin mô tả (tên, loại, giá trị...)

**Ví dụ:**

```
Layer: Trường học
├── Feature 1: Trường THCS Tân Phú
│   ├── Geometry: Point (105.8234, 21.0245)
│   └── Attributes:
│       ├── TenTruong: "THCS Tân Phú"
│       ├── LoaiTruong: "Trung học cơ sở"
│       ├── SoHocSinh: 500
│       └── NamThanhLap: 1995
├── Feature 2: Trường Tiểu học Hòa Bình
│   ├── Geometry: Point (105.8456, 21.0367)
│   └── Attributes: ...
└── ...
```

### 🔄 Quy trình chỉnh sửa

```
1. Xác định nơi chỉnh sửa
   ├── ArcGIS Pro (Offline, mạnh mẽ)
   └── ArcGIS Online (Online, tiện lợi)

2. Backup dữ liệu

3. Thực hiện chỉnh sửa
   ├── Attributes (Thông tin)
   ├── Geometry (Vị trí, hình dạng)
   └── Features (Thêm/xóa)

4. Validate (Kiểm tra)

5. Publish/Update lên web
```

---

## 2. CHỈNH SỬA LAYER TRONG ARCGIS PRO

### 🎯 Khi nào dùng ArcGIS Pro?

✅ **Nên dùng khi:**

- Chỉnh sửa hàng loạt (bulk edit)
- Thêm nhiều features cùng lúc
- Import từ Excel/CAD
- Thay đổi cấu trúc (thêm/xóa field)
- Xử lý dữ liệu phức tạp

❌ **Không cần dùng khi:**

- Sửa vài features đơn giản
- Chỉ cập nhật giá trị
- Không có ArcGIS Pro

### 📝 A. Chỉnh sửa Attributes

#### **Bước 1: Mở Attribute Table**

```
Cách 1:
1. Chuột phải vào layer
2. Chọn "Attribute Table"

Cách 2:
1. Click vào layer
2. Tab "Data" → "Attribute Table"

Phím tắt: Ctrl + T (khi đã chọn layer)
```

#### **Bước 2: Chỉnh sửa từng cell**

**Chỉnh sửa 1 giá trị:**

```
1. Click vào cell cần sửa
2. Nhập giá trị mới
3. Enter để lưu
4. Tab để chuyển sang cell kế bên
```

**Chỉnh sửa nhiều giá trị cùng lúc:**

```
1. Chọn nhiều rows:
   - Click vào row đầu
   - Shift + Click vào row cuối
   
2. Chuột phải vào tên cột cần sửa
   
3. Chọn "Calculate Field"
   
4. Trong hộp thoại:
   - Field Name: [Tên trường]
   - Expression: Nhập giá trị hoặc công thức
   
   Ví dụ:
   - Gán giá trị cố định: "Hoạt động"
   - Gán từ trường khác: !TenCu!
   - Tính toán: !DienTich! * 2
   
5. Click "OK"
```

**Ví dụ Calculate Field:**

```python
# Tạo mã tự động
# Trường mới: MaDiem
# Công thức:
f"TD_{!OBJECTID!:04d}"

# Kết quả: TD_0001, TD_0002, TD_0003...

# Chuyển đổi text → uppercase
!TenDiem!.upper()

# Kết quả: "trường học" → "TRƯỜNG HỌC"

# Tính toán điều kiện
def calculate(loai):
    if loai == "Giáo dục":
        return "Quan trọng"
    else:
        return "Bình thường"

calculate(!LoaiDiem!)
```

#### **Bước 3: Chỉnh sửa bằng SQL**

**Select by Attributes:**

```
1. Tab "Map" → "Select By Attributes"

2. Tạo câu query:
   - Where clause: TrangThai = 'Ngưng hoạt động'
   - Click "Run"
   
3. Các features thỏa điều kiện sẽ được chọn (highlight)

4. Chuột phải vào tên cột → Calculate Field
   - Chỉ áp dụng cho các row được chọn
```

**Ví dụ SQL queries:**

```sql
-- Chọn tất cả trường học
LoaiDiem = 'Giáo dục'

-- Chọn trường học còn hoạt động
LoaiDiem = 'Giáo dục' AND TrangThai = 'Hoạt động'

-- Chọn theo năm
NamThanhLap >= 2000 AND NamThanhLap <= 2010

-- Chọn theo tên (chứa từ khóa)
TenDiem LIKE '%Tân Phú%'

-- Chọn nhiều giá trị
LoaiDiem IN ('Giáo dục', 'Y tế', 'Hành chính')

-- Chọn null values
TenDiem IS NULL
```

### 🗺️ B. Chỉnh sửa Geometry

#### **Bước 1: Bật chế độ Edit**

```
1. Tab "Edit" → "Edit" (hoặc phím E)
2. Trong Edit toolbar, chọn layer cần edit
3. Select feature cần sửa
```

#### **Bước 2: Di chuyển Feature (Move)**

```
Đối với Point:
1. Click chọn point
2. Kéo thả đến vị trí mới
3. Hoặc nhập tọa độ chính xác:
   - Chuột phải → "Move To"
   - Nhập X, Y
   - Click "OK"

Đối với Polygon/Line:
1. Click chọn polygon
2. Kéo thả toàn bộ shape
3. Hoặc di chuyển từng vertex:
   - Click vào vertex (điểm đỉnh)
   - Kéo thả vertex đó
```

#### **Bước 3: Chỉnh sửa Shape**

**Thêm vertex:**

```
1. Click chọn polygon/line
2. Tab "Edit" → "Modify" → "Edit Vertices"
3. Di chuột đến cạnh cần thêm vertex
4. Click đúp để thêm vertex mới
5. Kéo vertex về vị trí mong muốn
```

**Xóa vertex:**

```
1. Edit Vertices mode
2. Click chọn vertex cần xóa
3. Delete
```

**Reshape (Thay đổi hình dạng):**

```
1. Tab "Edit" → "Modify" → "Reshape"
2. Vẽ line cắt qua polygon
3. Double-click để kết thúc
4. Polygon sẽ thay đổi theo line vừa vẽ
```

#### **Bước 4: Split/Merge Features**

**Split (Chia nhỏ):**

```
1. Select feature cần split
2. Tab "Edit" → "Modify" → "Split"
3. Vẽ line cắt qua feature
4. Double-click kết thúc
5. Feature sẽ tách thành 2 features mới
```

**Merge (Gộp):**

```
1. Select nhiều features cần merge
   - Ctrl + Click để chọn nhiều
2. Tab "Edit" → "Modify" → "Merge"
3. Chọn feature nào giữ attributes
4. Click "OK"
5. Nhiều features → 1 feature
```

#### **Bước 5: Lưu Edits**

```
⚠️ QUAN TRỌNG: Phải lưu sau khi edit

1. Tab "Edit" → "Save"
   Phím tắt: Ctrl + S
   
2. Hoặc discard (hủy):
   Tab "Edit" → "Discard"
```

### ➕ C. Thêm/Xóa Features

#### **Thêm Feature mới**

**Bằng tay:**

```
1. Tab "Edit" → "Create"
2. Chọn layer trong Create Features pane
3. Chọn công cụ vẽ:
   - Point: Click để đặt điểm
   - Line: Click nhiều lần, Double-click kết thúc
   - Polygon: Click nhiều lần, Double-click kết thúc
4. Nhập attributes vào bảng bên phải
5. Click "Finish"
```

**Copy từ feature khác:**

```
1. Select feature cần copy
2. Ctrl + C (Copy)
3. Ctrl + V (Paste)
4. Di chuyển feature mới về vị trí mong muốn
5. Sửa attributes
```

**Import từ Excel:**

```
1. Add Excel vào map
2. XY Table To Point
3. Copy Features từ event layer sang feature class
4. Lưu
```

#### **Xóa Features**

```
1. Select features cần xóa
   - Click từng cái
   - Hoặc Select By Attributes
   - Hoặc vẽ hình chọn (Select by Rectangle)
   
2. Delete
   
3. Save
```

⚠️ **Cảnh báo:** Xóa là vĩnh viễn! Nên backup trước.

---

## 3. CHỈNH SỬA LAYER TRÊN ARCGIS ONLINE

### 🌐 Khi nào dùng ArcGIS Online?

✅ **Nên dùng khi:**

- Sửa nhanh vài features
- Không có ArcGIS Pro
- Làm việc từ xa
- Cho phép nhiều người cùng edit

❌ **Hạn chế:**

- Không edit hàng loạt được
- Không thay đổi cấu trúc
- Cần internet

### 📝 A. Bật chế độ Edit

#### **Bước 1: Mở Map Viewer**

```
1. Đăng nhập https://www.arcgis.com
2. Content → Chọn layer
3. Click "Open in Map Viewer"
```

#### **Bước 2: Kiểm tra quyền Edit**

```
1. Click vào layer trong Contents
2. Click "..." → "Settings"
3. Tab "Editing":
   - ✅ Enable editing
   - Chọn quyền:
     ☑ Add features
     ☑ Delete features
     ☑ Update feature attributes
     ☑ Update feature geometry
4. Click "Save"
```

### ✏️ B. Chỉnh sửa Features

#### **Sửa Attributes:**

```
1. Click vào feature trên map
2. Pop-up hiện ra
3. Click nút "Edit" (biểu tượng bút chì)
4. Form chỉnh sửa hiện ra:
   - Sửa các giá trị cần thiết
   - Upload hình ảnh (nếu có)
5. Click "Update" để lưu
```

#### **Di chuyển Feature:**

```
1. Bật edit mode: Tab "Edit" → "Edit features"
2. Click chọn feature
3. Kéo thả về vị trí mới
4. Click "Save" ở góc trên
```

#### **Chỉnh sửa Shape:**

```
1. Edit mode
2. Click vào polygon/line
3. Các vertices hiện ra
4. Kéo vertex để thay đổi shape
5. Click "Save"
```

#### **Thêm Feature mới:**

```
1. Edit mode
2. Click "New feature"
3. Click vào map để đặt điểm
   (Hoặc vẽ polygon/line)
4. Nhập thông tin vào form
5. Click "Add"
```

#### **Xóa Feature:**

```
1. Click vào feature
2. Pop-up hiện ra
3. Click "Delete"
4. Confirm
```

### 🔧 C. Quản lý Fields (Trường dữ liệu)

**Thêm Field mới:**

```
1. Content → Chọn layer
2. Tab "Data"
3. Tab "Fields"
4. Click "Add" (nút +)
5. Thiết lập:
   - Field name: TenTruong (không dấu, không khoảng trắng)
   - Display name: Tên trường
   - Type: 
     • String (Text)
     • Integer (Số nguyên)
     • Double (Số thập phân)
     • Date (Ngày tháng)
   - Length: Độ dài (chỉ cho String)
   - Allow null values: Có cho phép để trống?
6. Click "Add new field"
```

**Xóa Field:**

```
⚠️ Cảnh báo: Không thể xóa field trên ArcGIS Online!

Cách khắc phục:
1. Download layer về dạng File Geodatabase
2. Xóa field trong ArcGIS Pro
3. Overwrite layer trên Online
```

**Sửa Field properties:**

```
Chỉ có thể sửa:
- Display name (Tên hiển thị)
- Description (Mô tả)
- Allow null values
- Default value

Không sửa được:
- Field name
- Data type
- Length
```

---

## 4. QUẢN LÝ ATTRIBUTES (THUỘC TÍNH)

### 📊 A. Các kiểu dữ liệu

| Type | Mô tả | Ví dụ | Khi nào dùng |
|------|-------|-------|--------------|
| **String** | Chữ | "Trường THCS Tân Phú" | Tên, mô tả, địa chỉ |
| **Integer** | Số nguyên | 500 | Số lượng, ID, năm |
| **Double** | Số thập phân | 123.45 | Diện tích, khoảng cách |
| **Date** | Ngày tháng | 2024-10-21 | Ngày thành lập, cập nhật |
| **GUID** | ID duy nhất | {ABC-123-...} | Mã định danh |

### 🔒 B. Thiết lập Constraints (Ràng buộc)

#### **Domain (Miền giá trị)**

Giới hạn giá trị chỉ được chọn từ danh sách.

**Tạo Domain trong ArcGIS Pro:**

```
1. Catalog → Chuột phải vào Geodatabase
2. "Domains" → "Add Domain"
3. Thiết lập:
   - Name: LoaiTruong_Domain
   - Description: Phân loại trường học
   - Field Type: Text
   - Domain Type: Coded Values
   
4. Thêm các giá trị:
   Code          Description
   ───────────────────────────
   MN            Mầm non
   TH            Tiểu học
   THCS          Trung học cơ sở
   THPT          Trung học phổ thông
   
5. Click "OK"
```

**Gán Domain cho Field:**

```
1. Mở Attribute Table
2. Tab "Fields" → Chọn field
3. Domain: Chọn domain vừa tạo
4. Click "Save"
```

**Kết quả:**

- Khi edit, chỉ chọn được từ dropdown
- Không thể nhập giá trị khác

#### **Default Value (Giá trị mặc định)**

```
1. Tab "Fields" → Chọn field
2. Default Value: Nhập giá trị
   Ví dụ: "Hoạt động"
3. Save
```

Khi thêm feature mới, field tự động có giá trị này.

#### **Allow Null Values**

```
1. Tab "Fields" → Chọn field
2. ☑ Allow NULL values
   - Checked: Được để trống
   - Unchecked: BẮT BUỘC nhập
3. Save
```

### 🔄 C. Import/Export Attributes

#### **Export sang Excel:**

```
Trong ArcGIS Pro:
1. Chuột phải layer → Data → Export Table
2. Output: Chọn vị trí lưu .xlsx
3. Click "OK"
4. Mở file Excel → Chỉnh sửa

Trong ArcGIS Online:
1. Tab "Data"
2. Click "Export Data"
3. Chọn "CSV" hoặc "Excel"
4. Download
```

#### **Import từ Excel (Update attributes):**

```
1. Export layer hiện tại sang Excel
2. Sửa trong Excel (giữ nguyên cột OBJECTID)
3. Save Excel
4. Trong ArcGIS Pro:
   - Add Excel vào map
   - Geoprocessing → "Join Field"
     • Input: Layer cần update
     • Join Field: OBJECTID
     • Join Table: Excel
     • Join Table Field: OBJECTID
   - Calculate Field từ joined table
   - Remove Join
```

---

## 5. CHỈNH SỬA GEOMETRY (HÌNH HỌC)

### 📍 A. Chỉnh sửa Points

#### **Di chuyển điểm chính xác:**

```
1. Select point
2. Chuột phải → "Move To"
3. Nhập tọa độ:
   - X (Kinh độ): 105.82345
   - Y (Vĩ độ): 21.03456
4. Click "OK"
```

#### **Snap to location (Dính vào vị trí):**

```
1. Tab "Edit" → "Snapping"
2. Bật các tùy chọn:
   ☑ Point
   ☑ Vertex
   ☑ Edge
3. Khi di chuyển điểm, sẽ tự động dính vào:
   - Điểm khác
   - Đỉnh của polygon
   - Cạnh của line
```

### 📐 B. Chỉnh sửa Polygons

#### **Vẽ chính xác bằng Constraints:**

```
Parallel (Song song):
1. Tab "Edit" → "Constraints" → "Parallel"
2. Vẽ cạnh mới → Tự động song song với cạnh cũ

Perpendicular (Vuông góc):
1. Constraints → "Perpendicular"
2. Vẽ cạnh vuông góc

Distance (Khoảng cách cố định):
1. Constraints → "Distance"
2. Nhập khoảng cách: 100 (meters)
3. Vẽ → Cạnh có độ dài chính xác 100m

Angle (Góc cố định):
1. Constraints → "Direction"
2. Nhập góc: 45°
3. Vẽ → Cạnh có góc 45°
```

#### **Buffer (Tạo vùng đệm):**

```
1. Select polygon
2. Tab "Edit" → "Modify" → "Buffer"
3. Nhập:
   - Distance: 100
   - Unit: Meters
   - Type: 
     • Full: Vòng quanh
     • Left: Bên trái
     • Right: Bên phải
4. Click "Buffer"
```

#### **Clip (Cắt):**

```
1. Vẽ polygon cần giữ lại
2. Select polygon cần clip
3. Tab "Edit" → "Modify" → "Clip"
4. Phần nằm ngoài sẽ bị cắt bỏ
```

### 📏 C. Đo và Tính toán

#### **Tính diện tích tự động:**

```
1. Thêm field mới: DienTich (Double)
2. Chuột phải vào field → "Calculate Geometry"
3. Property: Area
4. Units: Square Meters / Square Kilometers
5. Click "OK"
```

#### **Tính độ dài (cho line):**

```
1. Thêm field: DoDai (Double)
2. Calculate Geometry
3. Property: Length
4. Units: Meters / Kilometers
```

#### **Tính tọa độ trung tâm:**

```
// Tính X (Kinh độ)
1. Thêm field: KinhDo (Double)
2. Calculate Geometry
3. Property: X Coordinate of Centroid

// Tính Y (Vĩ độ)
1. Thêm field: ViDo (Double)
2. Calculate Geometry
3. Property: Y Coordinate of Centroid
```

---

## 6. THÊM/XÓA FEATURES

### ➕ A. Thêm Features hàng loạt

#### **Từ Excel:**

```
1. Chuẩn bị Excel với cấu trúc đầy đủ
2. XY Table To Point
3. Append vào layer chính:
   - Geoprocessing → "Append"
   - Input: Layer mới từ Excel
   - Target: Layer chính
   - Schema Type: NO_TEST
   - Click "Run"
```

#### **Từ file khác:**

```
1. Add Data → Chọn file (.shp, .kml, .geojson)
2. Append vào layer chính
```

### ❌ B. Xóa Features hàng loạt

```
⚠️ Cảnh báo: Backup trước khi xóa!

1. Select By Attributes:
   Ví dụ: TrangThai = 'Ngưng hoạt động'
   
2. Kiểm tra số lượng đã chọn
   
3. Delete
   
4. Save
```

### 🔄 C. Replace toàn bộ dữ liệu

**Cách 1: Truncate + Append**

```
1. Backup layer
2. Truncate (Xóa toàn bộ):
   - Select All (Ctrl + A trong Attribute Table)
   - Delete
3. Append data mới
```

**Cách 2: Overwrite layer**

```
1. Publish layer mới với tên giống hệt
2. Chọn "Overwrite existing layer"
3. Publish
```

---

## 7. BEST PRACTICES

### ✅ Quy tắc vàng

1. **LUÔN BACKUP trước khi edit**

   ```
   - Export sang File Geodatabase
   - Hoặc copy layer trước khi sửa
   ```

2. **Đặt tên chuẩn**

   ```
   Field names:
   ✅ TenDiem, LoaiDiem, NgayTao
   ❌ Tên điểm, Loại, ngày tạo (có dấu, khoảng trắng)
   ```

3. **Validate sau khi edit**

   ```
   - Check Geometry
   - Repair Geometry
   - Kiểm tra null values
   ```

4. **Document changes**

   ```
   Tạo field: NgaySua, NguoiSua
   Tự động track: Enable Editor Tracking
   ```

### 🔒 Bảo mật

```
1. Phân quyền rõ ràng:
   - Admin: Full quyền
   - Editor: Chỉ edit
   - Viewer: Chỉ xem

2. Version control:
   - Sử dụng Versioning (Enterprise Geodatabase)
   - Lưu history

3. Regular backup:
   - Hàng ngày: Nếu thay đổi nhiều
   - Hàng tuần: Nếu ít thay đổi
```

### 🎯 Tối ưu hiệu suất

```
1. Indexes:
   - Tạo index cho fields thường query
   - Rebuild index định kỳ

2. Geometry:
   - Simplify geometry nếu quá phức tạp
   - Remove duplicate vertices

3. Attributes:
   - Xóa fields không dùng
   - Sử dụng coded values thay vì text tự do
```

### 📊 Quality Control Checklist

```
☑ Không có null values trong required fields
☑ Không có duplicate features
☑ Geometry hợp lệ (không self-intersect)
☑ Attributes trong domain/range hợp lệ
☑ Coordinate system đúng
☑ Topology rules không vi phạm
☑ Pop-up hiển thị đúng
☑ Symbology rõ ràng
```

---

## 8. TROUBLESHOOTING

### ❌ Không edit được

**Nguyên nhân:**

- Layer không có quyền edit
- Layer đang bị lock
- Không ở trong edit session

**Giải pháp:**

```
1. Kiểm tra quyền: Layer Settings → Editing
2. Stop edit sessions khác
3. Bật edit mode: Tab Edit → Edit
```

### ❌ Calculate Field lỗi

**Lỗi:** "ERROR 000539: Invalid field"

**Giải pháp:**

```
1. Kiểm tra tên field đúng không
2. Kiểm tra kiểu dữ liệu phù hợp
3. Syntax đúng chưa:
   - Python: !FieldName!
   - Arcade: $feature.FieldName
```

### ❌ Geometry không hợp lệ

**Lỗi:** "Geometry is invalid"

**Giải pháp:**

```
1. Geoprocessing → "Check Geometry"
2. Xem lỗi cụ thể
3. "Repair Geometry"
4. Nếu vẫn lỗi:
   - Delete vertex gây lỗi
   - Vẽ lại feature
```

### ❌ Append thất bại

**Lỗi:** "Schema mismatch"

**Giải pháp:**

```
1. Kiểm tra schema 2 layers giống nhau
2. Field Mapping:
   - Map fields tương ứng
   - Ignore fields không cần
3. Schema Type: NO_TEST (nếu chắc chắn)
```

---

## 9. TÀI LIỆU THAM KHẢO

### 📚 Links hữu ích

- ArcGIS Pro Edit Tutorial: <https://pro.arcgis.com/en/pro-app/latest/help/editing/>
- Field Calculator: <https://pro.arcgis.com/en/pro-app/latest/help/data/tables/calculate-field-examples.htm>
- Geometry Editor: <https://pro.arcgis.com/en/pro-app/latest/help/editing/modify-features.htm>

### 🎓 Video tutorials

- Esri Training: <https://www.esri.com/training/>
- YouTube: "ArcGIS Pro Editing"

### 💬 Community

- GIS Stack Exchange: <https://gis.stackexchange.com/>
- Esri Community: <https://community.esri.com/>

---

**Cập nhật lần cuối**: 21/10/2025
**Phiên bản**: 1.0
**Tác giả**: Ho Doan
