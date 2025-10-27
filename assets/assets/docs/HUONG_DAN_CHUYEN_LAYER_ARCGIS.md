<!-- spell-checker: disable -->
# HƯỚNG DẪN CHUYỂN LAYER TỪ ARCGIS PRO → ARCGIS ONLINE → WEB

## 📋 MỤC LỤC

1. [Tổng quan quy trình](#1-tổng-quan-quy-trình)
2. [Chuẩn bị dữ liệu trong ArcGIS Pro](#2-chuẩn-bị-dữ-liệu-trong-arcgis-pro)
3. [Publish lên ArcGIS Online](#3-publish-lên-arcgis-online)
4. [Cập nhật (Overwrite) Web Layer](#4-cập-nhật-overwrite-web-layer)
5. [Tích hợp vào Web App](#5-tích-hợp-vào-web-app)
6. [Xử lý sự cố thường gặp](#6-xử-lý-sự-cố-thường-gặp)

---

## 1. TỔNG QUAN QUY TRÌNH

### 🔄 Sơ đồ luồng dữ liệu

```txt
┌─────────────────┐
│  Dữ liệu Excel  │
│   hoặc CAD      │
└────────┬────────┘
         │
         ▼
┌─────────────────────┐
│   ArcGIS Pro        │ ← Xử lý và biên tập dữ liệu
│                     │   + Import từ Excel/CAD
│   - Tạo Layer      │   + Tạo symbology
│   - Symbology      │   + Thiết lập pop-up
│   - Pop-up         │   + Kiểm tra dữ liệu
└────────┬────────────┘
         │
         ▼ Publish
┌─────────────────────┐
│  ArcGIS Online      │ ← Lưu trữ và chia sẻ
│                     │   + Quản lý quyền
│   - Feature Layer  │   + Lưu trữ dữ liệu
│   - Map Service    │   + REST API
└────────┬────────────┘
         │
         ▼ Integrate
┌─────────────────────┐
│   Flutter Web App   │ ← Hiển thị cho người dùng
│                     │   + Load layer qua URL
│   - Display Map    │   + Tương tác với map
│   - Query Data     │   + Lọc và tìm kiếm
│   - Statistics     │   + Thống kê dữ liệu
└─────────────────────┘
```

### ⏱️ Thời gian ước tính

- **Lần đầu**: 2-3 giờ (bao gồm học và thử nghiệm)
- **Lần sau**: 30-45 phút (đã quen thuộc)

---

## 2. CHUẨN BỊ DỮ LIỆU TRONG ARCGIS PRO

### 📝 Bước 1: Mở ArcGIS Pro và tạo Project mới

1. Mở **ArcGIS Pro**
2. Click **New** → **Map**
3. Đặt tên project: `[TenXa]_WebGIS_[Ngay]`
   - Ví dụ: `XaTanPhu_WebGIS_20251021`
4. Chọn vị trí lưu project

### 📊 Bước 2: Import dữ liệu từ Excel

#### A. Nếu dữ liệu là **Điểm** (Point)

##### **2.1. Chuẩn bị file Excel**

File Excel phải có cấu trúc như sau:

| ID | TenDiem | MoTa | KinhDo | ViDo | LoaiDiem | TrangThai |
|----|---------|------|---------|------|----------|-----------|
| 1 | Trường học A | Trường THCS | 105.8234 | 21.0245 | Giáo dục | Hoạt động |
| 2 | Trạm y tế B | Trạm y tế xã | 105.8456 | 21.0367 | Y tế | Hoạt động |

**Lưu ý quan trọng:**

- Tên cột **KHÔNG** được có dấu, khoảng trắng, ký tự đặc biệt
- Tọa độ phải là số thập phân (Decimal Degrees)
- Hệ tọa độ: WGS 1984 (EPSG:4326) hoặc VN-2000

##### **2.2. Import vào ArcGIS Pro**

```txt
1. Click tab "Map" → "Add Data" → "Data"
2. Chọn file Excel (.xlsx)
3. Chọn Sheet cần import
4. Click "OK"
```

##### **2.3. Tạo Feature Layer từ XY Data**

```txt
1. Mở "Geoprocessing" (Ctrl + Alt + G)
2. Tìm công cụ: "XY Table To Point"
3. Thiết lập:
   - Input Table: [Sheet Excel vừa import]
   - X Field: KinhDo
   - Y Field: ViDo
   - Coordinate System: WGS 1984 (WKID: 4326)
   - Output Feature Class: 
     Tên: [TenLayer]_Points
     Vị trí: Project Geodatabase
4. Click "Run"
```

#### B. Nếu dữ liệu là **Polygon** (Vùng)

##### **2.1. Từ file CAD/SHP**

```txt
1. Add Data → Chọn file .dwg / .shp
2. Chuột phải vào layer → Data → Export Features
3. Output Feature Class: Đặt tên và chọn vị trí
4. Click "OK"
```

##### **2.2. Vẽ thủ công**

```txt
1. Tab "Edit" → "Create"
2. Chọn layer cần vẽ
3. Sử dụng các công cụ:
   - Polygon: Vẽ đa giác
   - Rectangle: Vẽ hình chữ nhật
   - Circle: Vẽ hình tròn
4. Nhập thông tin vào Attribute Table
```

### 🎨 Bước 3: Thiết lập Symbology (Ký hiệu)

#### **3.1. Mở Symbology**

```txt
1. Chuột phải vào layer → "Symbology"
2. Hoặc: Click layer → Tab "Appearance" → "Symbology"
```

#### **3.2. Chọn kiểu hiển thị**

#### **Hiển thị đơn giản** (Single Symbol)

- Tất cả feature cùng một màu/ký hiệu
- Dùng cho: Layer đồng nhất

```txt
1. Primary symbology: Single Symbol
2. Click vào symbol → chọn màu, kích thước
3. Click "Apply"
```

#### **Phân loại theo giá trị** (Unique Values)

- Mỗi loại một màu/ký hiệu khác nhau
- Dùng cho: Phân loại theo thuộc tính

```txt
1. Primary symbology: Unique Values
2. Field 1: Chọn trường phân loại (VD: LoaiDiem)
3. Click "Add All Values"
4. Tùy chỉnh màu sắc cho từng loại:
   - Click vào symbol của mỗi loại
   - Chọn màu, biểu tượng phù hợp
5. Click "Apply"
```

#### **Phân cấp theo khoảng giá trị** (Graduated Colors)

- Dùng cho dữ liệu số
- Ví dụ: Dân số, diện tích

```txt
1. Primary symbology: Graduated Colors
2. Field: Chọn trường số (VD: DanSo)
3. Method: 
   - Natural Breaks (Jenks): Khuyến nghị
   - Equal Interval: Khoảng đều
   - Quantile: Số lượng đều
4. Classes: Chọn số cấp (thường 5)
5. Color scheme: Chọn dải màu
6. Click "Apply"
```

#### **3.3. Ví dụ thiết lập Symbology cho điểm**

```dart
// Phân loại theo loại công trình
Loại           Symbol          Màu        Kích thước
─────────────────────────────────────────────────────
Trường học     🏫              #FF6B6B    16px
Trạm y tế      🏥              #4ECDC4    16px  
Chợ            🏪              #FFE66D    16px
Công viên      🌳              #95E1D3    16px
```

### 💬 Bước 4: Cấu hình Pop-up

Pop-up là cửa sổ hiển thị thông tin khi click vào feature.

#### **4.1. Mở Configure Pop-ups**

```txt
1. Chuột phải vào layer → "Configure Pop-ups"
```

#### **4.2. Thiết lập Title**

```txt
1. Pop-up Title: 
   Nhập: {TenDiem}
   
   Hoặc kết hợp nhiều trường:
   {TenDiem} - {LoaiDiem}
```

**4.3. Thiết lập nội dung (Content)**

**Cách 1: A list of field attributes (Đơn giản)**

```txt
1. Chọn: "A list of field attributes"
2. Click "Configure attributes"
3. Chọn các trường cần hiển thị:
   ✓ TenDiem → Display Name: "Tên điểm"
   ✓ MoTa → Display Name: "Mô tả"
   ✓ LoaiDiem → Display Name: "Loại điểm"
   ✓ TrangThai → Display Name: "Trạng thái"
   ✗ OBJECTID (Bỏ chọn)
   ✗ Shape (Bỏ chọn)
4. Sắp xếp thứ tự hiển thị
5. Click "OK"
```

**Cách 2: Custom HTML (Nâng cao)**

```html
1. Chọn: "A custom attribute display"
2. Nhập HTML:

<div style="font-family: Arial; padding: 10px;">
  <h3 style="color: #0078D4; margin: 0 0 10px 0;">
    {TenDiem}
  </h3>
  
  <table style="width: 100%; border-collapse: collapse;">
    <tr>
      <td style="padding: 5px; font-weight: bold;">Loại:</td>
      <td style="padding: 5px;">{LoaiDiem}</td>
    </tr>
    <tr style="background-color: #f5f5f5;">
      <td style="padding: 5px; font-weight: bold;">Trạng thái:</td>
      <td style="padding: 5px;">{TrangThai}</td>
    </tr>
    <tr>
      <td style="padding: 5px; font-weight: bold;">Mô tả:</td>
      <td style="padding: 5px;">{MoTa}</td>
    </tr>
  </table>
</div>
```

**4.4. Thêm Media (Hình ảnh, biểu đồ)**

```txt
1. Click "Add content"
2. Chọn:
   - Image: Thêm hình ảnh
   - Chart: Thêm biểu đồ
   - Text: Thêm text tùy chỉnh
3. Configure và click "OK"
```

### ✅ Bước 5: Kiểm tra dữ liệu

**5.1. Mở Attribute Table**

```txt
1. Chuột phải vào layer → "Attribute Table"
2. Kiểm tra:
   - Có dữ liệu null không?
   - Các giá trị có đúng không?
   - Có lỗi chính tả không?
```

**5.2. Validate Geometry**

```txt
1. Geoprocessing → "Check Geometry"
2. Input: Layer cần kiểm tra
3. Click "Run"
4. Nếu có lỗi → Dùng "Repair Geometry"
```

**5.3. Test Pop-up**

```txt
1. Click vào feature trên map
2. Kiểm tra pop-up hiển thị đúng không
3. Nếu sai → quay lại Bước 4 sửa
```

---

## 3. PUBLISH LÊN ARCGIS ONLINE

### 🚀 Bước 1: Đăng nhập ArcGIS Online

**1.1. Sign in trong ArcGIS Pro**

```txt
1. Click vào tên user ở góc trên bên phải
2. Nếu chưa đăng nhập → Click "Sign In"
3. Nhập:
   - ArcGIS Online URL: https://www.arcgis.com
   - Username: [your_username]
   - Password: [your_password]
4. Click "Sign In"
```

**1.2. Kiểm tra quyền**

Đảm bảo tài khoản có quyền:

- ✅ Create content
- ✅ Publish hosted feature layers
- ✅ Share content

### 📤 Bước 2: Share As Web Layer

**2.1. Chọn layer cần publish**

```txt
1. Trong "Contents" pane, click chọn layer
2. Tab "Share" → "Web Layer"
```

**2.2. Thiết lập Web Layer**

```txt
┌─────────────────────────────────────────────┐
│ Share As Web Layer                          │
├─────────────────────────────────────────────┤
│                                             │
│ Layer Type: Feature                         │
│ ○ Feature   ○ Vector Tile   ○ Tile         │
│                                             │
│ Name: [TenLayer]_[TenXa]                    │
│ Ví dụ: TruongHoc_XaTanPhu                   │
│                                             │
│ Summary: (Bắt buộc)                         │
│ Layer bản đồ các trường học thuộc xã Tân Phú│
│                                             │
│ Tags: (Bắt buộc)                            │
│ trường học, giáo dục, tân phú, bản đồ       │
│                                             │
│ Folder: (Tùy chọn)                          │
│ XaTanPhu                                    │
│                                             │
│ Share with:                                 │
│ □ Owner                                     │
│ ☑ Organization                              │
│ ☑ Everyone (public)                         │
│ □ Groups: [Chọn group nếu có]               │
│                                             │
└─────────────────────────────────────────────┘
```

**Giải thích các tùy chọn:**

- **Layer Type**:
  - `Feature`: Cho dữ liệu vector (điểm, đường, vùng) - **Chọn này**
  - `Vector Tile`: Cho basemap
  - `Tile`: Cho hình ảnh

- **Share with**:
  - `Owner`: Chỉ mình bạn thấy
  - `Organization`: Tổ chức của bạn thấy
  - `Everyone`: Công khai - **Chọn này nếu muốn public**

**2.3. Cấu hình nâng cao (Configuration)**

Click vào tab **"Configuration"**:

```txt
┌─────────────────────────────────────────────┐
│ Configuration                               │
├─────────────────────────────────────────────┤
│                                             │
│ ☑ Copy all data (Copy source data)         │
│   Khuyến nghị: Chọn                         │
│                                             │
│ Feature Layer Properties:                   │
│ ☑ Enable editing                            │
│   □ Add   □ Delete   □ Update               │
│   → Chọn nếu cho phép chỉnh sửa trên web    │
│                                             │
│ ☑ Enable Sync                               │
│   → Cho phép offline sync (mobile)          │
│                                             │
│ ☑ Keep track of created and updated        │
│   → Theo dõi ai tạo, ai sửa, khi nào        │
│                                             │
│ Caching:                                    │
│ ○ Automatic                                 │
│ ○ Manual                                    │
│ ○ None                                      │
│   Khuyến nghị: Automatic                    │
│                                             │
└─────────────────────────────────────────────┘
```

**Khuyến nghị cấu hình:**

| Tùy chọn | Nên chọn | Lý do |
|----------|----------|-------|
| Copy all data | ✅ Yes | Đảm bảo dữ liệu được copy lên online |
| Enable editing | ⚠️ Tùy | Chỉ bật nếu cho phép user edit |
| Enable Sync | ✅ Yes | Hỗ trợ offline mobile |
| Track changes | ✅ Yes | Biết ai sửa gì, khi nào |
| Caching | ✅ Automatic | Tăng tốc độ load |

**2.4. Analyze và Publish**

```txt
1. Click nút "Analyze"
   → ArcGIS sẽ kiểm tra lỗi
   
2. Xử lý Errors và Warnings:
   - 🔴 Errors: BẮT BUỘC phải sửa
   - 🟡 Warnings: Nên sửa nhưng không bắt buộc
   
3. Sửa lỗi nếu có
   
4. Click "Publish"
   
5. Đợi quá trình upload (2-10 phút tùy dung lượng)
```

**2.5. Xử lý lỗi thường gặp**

| Lỗi | Nguyên nhân | Giải pháp |
|-----|-------------|-----------|
| "Layer has no coordinate system" | Chưa set CRS | Set coordinate system cho layer |
| "Summary is required" | Chưa nhập summary | Nhập mô tả vào trường Summary |
| "Tags are required" | Chưa có tags | Thêm ít nhất 1 tag |
| "Invalid field names" | Tên trường có ký tự đặc biệt | Đổi tên trường (không dấu, không khoảng trắng) |

### ✅ Bước 3: Kiểm tra trên ArcGIS Online

**3.1. Mở ArcGIS Online**

```txt
1. Mở trình duyệt
2. Truy cập: https://www.arcgis.com
3. Sign In với tài khoản đã dùng
```

**3.2. Tìm layer vừa publish**

```txt
1. Click "Content"
2. Tìm layer theo tên
3. Click vào layer để xem chi tiết
```

**3.3. Xem thông tin layer**

```txt
┌─────────────────────────────────────────────┐
│ [Tên Layer]                                 │
├─────────────────────────────────────────────┤
│                                             │
│ 📊 Overview                                 │
│    - Title, Summary, Tags, Description      │
│                                             │
│ 📈 Data                                     │
│    - Xem attribute table                    │
│    - Query dữ liệu                          │
│                                             │
│ 🎨 Visualization                            │
│    - Preview bản đồ                         │
│    - Kiểm tra symbology                     │
│                                             │
│ ⚙️ Settings                                 │
│    - Feature Layer settings                 │
│    - Editing permissions                    │
│                                             │
└─────────────────────────────────────────────┘
```

**3.4. Copy Service URL**

**Quan trọng:** Cần URL này để tích hợp vào Web App

```txt
1. Tab "Overview"
2. Kéo xuống phần "Layers"
3. Nhìn thấy URL dạng:
   
   https://services.arcgis.com/[OrgID]/arcgis/rest/services/[LayerName]/FeatureServer/0
   
4. Copy URL này → Lưu vào Notepad
```

**Ví dụ URL:**

```txt
https://services.arcgis.com/abc123xyz/arcgis/rest/services/TruongHoc_XaTanPhu/FeatureServer/0
```

---

## 4. CẬP NHẬT (OVERWRITE) WEB LAYER

Khi dữ liệu thay đổi, cần cập nhật lại layer đã publish.

### 🔄 Phương pháp 1: Overwrite trong ArcGIS Pro (Khuyến nghị)

**4.1. Cập nhật dữ liệu trong ArcGIS Pro**

```txt
1. Mở project ArcGIS Pro chứa layer gốc
2. Chỉnh sửa dữ liệu:
   - Thêm/xóa/sửa features
   - Cập nhật attributes
   - Thay đổi symbology (nếu cần)
3. Lưu project
```

**4.2. Overwrite Layer**

```txt
1. Click vào layer cần update
2. Tab "Share" → "Web Layer"
3. Tab "General":
   - Name: GIỮ NGUYÊN tên layer cũ
   - ☑ Overwrite existing layer
   
4. Click "Analyze"
5. Click "Publish"
6. Chọn layer cần overwrite từ danh sách
7. Confirm → Click "Yes, Overwrite"
8. Đợi hoàn thành
```

**⚠️ Lưu ý quan trọng khi Overwrite:**

| Điều kiện | Yêu cầu |
|-----------|---------|
| Tên layer | Phải giống hệt tên cũ |
| Schema (Cấu trúc trường) | Không được thay đổi tên, kiểu dữ liệu các trường |
| Coordinate System | Phải giữ nguyên |
| Geometry Type | Không đổi (Point vẫn là Point) |

**Có thể thay đổi:**

- ✅ Số lượng features (thêm/bớt)
- ✅ Giá trị trong các trường
- ✅ Symbology
- ✅ Pop-up configuration

**Không được thay đổi:**

- ❌ Tên trường
- ❌ Kiểu dữ liệu trường (Text → Number)
- ❌ Hệ tọa độ
- ❌ Geometry type

### 🔄 Phương pháp 2: Cập nhật qua REST API

Dùng khi cần tự động hóa hoặc cập nhật từ script.

**4.1. Lấy Token**

```python
import requests

# Endpoint
url = "https://www.arcgis.com/sharing/rest/generateToken"

# Credentials
data = {
    'username': 'your_username',
    'password': 'your_password',
    'referer': 'https://www.arcgis.com',
    'f': 'json'
}

response = requests.post(url, data=data)
token = response.json()['token']
print(f"Token: {token}")
```

**4.2. Apply Edits**

```python
# Service URL
service_url = "https://services.arcgis.com/.../FeatureServer/0"

# Thêm feature mới
add_features = [{
    "geometry": {"x": 105.8234, "y": 21.0245},
    "attributes": {
        "TenDiem": "Trường mới",
        "LoaiDiem": "Giáo dục",
        "TrangThai": "Hoạt động"
    }
}]

# Apply edits
edit_url = f"{service_url}/applyEdits"
params = {
    'adds': json.dumps(add_features),
    'token': token,
    'f': 'json'
}

response = requests.post(edit_url, data=params)
print(response.json())
```

### 📅 Phương pháp 3: Scheduled Update (Tự động)

Nếu dữ liệu cập nhật định kỳ từ Excel:

**4.1. Chuẩn bị**

```txt
1. Lưu file Excel vào thư mục cố định
2. Tạo Python script để:
   - Đọc Excel
   - Connect tới ArcGIS Online
   - Update features
3. Lên lịch chạy script (Windows Task Scheduler)
```

**4.2. Script Python mẫu**

```python
from arcgis.gis import GIS
import pandas as pd

# Connect to ArcGIS Online
gis = GIS("https://www.arcgis.com", "username", "password")

# Read Excel
df = pd.read_excel("data.xlsx")

# Get layer
item = gis.content.get("LAYER_ID")
layer = item.layers[0]

# Truncate old data
layer.delete_features(where="1=1")

# Add new data
features = []
for _, row in df.iterrows():
    feature = {
        "geometry": {"x": row['KinhDo'], "y": row['ViDo']},
        "attributes": {
            "TenDiem": row['TenDiem'],
            "LoaiDiem": row['LoaiDiem'],
            # ... other fields
        }
    }
    features.append(feature)

layer.edit_features(adds=features)
print("Updated successfully!")
```

---

## 5. TÍCH HỢP VÀO WEB APP

### 🌐 Bước 1: Lấy thông tin Layer

Sau khi publish, cần 2 thông tin:

1. **Service URL**:

   ```txt
   https://services.arcgis.com/[OrgID]/arcgis/rest/services/[LayerName]/FeatureServer/0
   ```

2. **Portal Item ID** (Tùy chọn):

   ```txt
   Trên ArcGIS Online → Tab Settings → Item ID
   ```

### 💻 Bước 2: Thêm Layer vào Code

#### A. Tạo file config cho layer mới

Tạo file: `example/lib/src/features/home/widgets/tabs/models/layer_config.dart`

```dart
class LayerConfig {
  final String id;
  final String name;
  final String url;
  final String? iconAsset;
  final List<LayerField> fields;

  const LayerConfig({
    required this.id,
    required this.name,
    required this.url,
    this.iconAsset,
    required this.fields,
  });
}

class LayerField {
  final String name;
  final String alias;
  final String type;
  
  const LayerField({
    required this.name,
    required this.alias,
    required this.type,
  });
}

// Định nghĩa các layer
class AppLayers {
  static const truongHoc = LayerConfig(
    id: 'truong_hoc',
    name: 'Trường học',
    url: 'https://services.arcgis.com/.../TruongHoc/FeatureServer/0',
    iconAsset: 'assets/map_truonghoc.png',
    fields: [
      LayerField(name: 'TenDiem', alias: 'Tên trường', type: 'string'),
      LayerField(name: 'LoaiDiem', alias: 'Loại', type: 'string'),
      LayerField(name: 'TrangThai', alias: 'Trạng thái', type: 'string'),
    ],
  );
  
  static const tramYTe = LayerConfig(
    id: 'tram_yte',
    name: 'Trạm y tế',
    url: 'https://services.arcgis.com/.../TramYTe/FeatureServer/0',
    iconAsset: 'assets/map_tramyte.png',
    fields: [
      LayerField(name: 'TenTram', alias: 'Tên trạm', type: 'string'),
      LayerField(name: 'SoGiuong', alias: 'Số giường', type: 'integer'),
    ],
  );
  
  // Danh sách tất cả layers
  static const allLayers = [
    truongHoc,
    tramYTe,
    // Thêm các layer khác ở đây
  ];
}
```

#### B. Load layer trong code

Trong file quản lý layer (có thể là Bloc hoặc Service):

```dart
import 'package:arcgis_sdk/arcgis_sdk_web.dart';

class LayerService {
  Future<FeatureLayer> loadLayer(LayerConfig config) async {
    final layer = FeatureLayer(
      FeatureLayerProperties(
        url: config.url,
        outFields: ['*'],
        popupEnabled: true,
        popupTemplate: PopupTemplate(
          PopupTemplateProperties(
            title: '{${config.fields[0].name}}',
            content: [
              FieldsContent(
                FieldsContentProperties(
                  fieldInfos: config.fields
                      .map((field) => FieldInfo(
                            FieldInfoProperties(
                              fieldName: field.name,
                              label: field.alias,
                            ),
                          ))
                      .toList(),
                ),
              ),
            ],
          ),
        ),
      ),
    );
    
    return layer;
  }
  
  void addLayerToMap(MapView mapView, FeatureLayer layer) {
    mapView.map?.add(layer);
  }
}
```

#### C. Sử dụng trong UI

Trong tab Layer (`layer.dart`):

```dart
class _LayerTabState extends State<LayerTab> {
  final layers = <FeatureLayer>[];
  
  @override
  void initState() {
    super.initState();
    _loadAllLayers();
  }
  
  Future<void> _loadAllLayers() async {
    final layerService = LayerService();
    
    for (final config in AppLayers.allLayers) {
      final layer = await layerService.loadLayer(config);
      setState(() {
        layers.add(layer);
      });
    }
  }
  
  @override
  Widget build(BuildContext context) {
    return ListView.builder(
      itemCount: AppLayers.allLayers.length,
      itemBuilder: (context, index) {
        final config = AppLayers.allLayers[index];
        return ListTile(
          leading: config.iconAsset != null
              ? Image.asset(config.iconAsset!, width: 24, height: 24)
              : const Icon(Icons.layers),
          title: Text(config.name),
          trailing: Switch(
            value: layers.length > index,
            onChanged: (value) {
              if (value) {
                // Add layer to map
                widget.controller.mapView.map?.add(layers[index]);
              } else {
                // Remove layer from map
                widget.controller.mapView.map?.remove(layers[index]);
              }
              setState(() {});
            },
          ),
        );
      },
    );
  }
}
```

### 🔍 Bước 3: Test layer mới

```txt
1. Chạy app: flutter run -d chrome
2. Mở tab "Lớp bản đồ"
3. Bật layer mới thêm
4. Kiểm tra:
   - ✅ Layer hiển thị đúng vị trí?
   - ✅ Symbology đúng như trong ArcGIS Pro?
   - ✅ Click vào feature → Pop-up hiển thị?
   - ✅ Dữ liệu đầy đủ?
```

---

## 6. XỬ LÝ SỰ CỐ THƯỜNG GẶP

### ❌ Layer không hiển thị

**Nguyên nhân có thể:**

1. **URL sai**

   ```txt
   Giải pháp:
   - Kiểm tra lại URL từ ArcGIS Online
   - Đảm bảo có "/0" hoặc "/1" cuối URL (layer index)
   - Test URL bằng cách mở trong browser:
     https://services.../FeatureServer/0?f=json
   ```

2. **Layer không public**

   ```txt
   Giải pháp:
   - Vào ArcGIS Online
   - Settings → Share with Everyone
   ```

3. **CORS error**

   ```txt
   Giải pháp:
   - Thêm domain vào whitelist của ArcGIS Online
   - Hoặc dùng proxy
   ```

### ❌ Symbology không đúng

**Nguyên nhân:**

- Symbology không được copy khi publish

**Giải pháp:**

```txt
Cách 1: Set lại symbology trên ArcGIS Online
1. Content → Chọn layer
2. Tab Visualization
3. Change Style
4. Thiết lập lại symbology

Cách 2: Set symbology trong code
// Trong Flutter code
final renderer = SimpleRenderer(
  SimpleRendererProperties(
    symbol: SimpleMarkerSymbol(
      SimpleMarkerSymbolProperties(
        color: [255, 0, 0, 255],
        size: '12px',
        style: 'circle',
      ),
    ),
  ),
);

layer.renderer = renderer;
```

### ❌ Pop-up không hiển thị

**Giải pháp:**

```dart
// Đảm bảo popupEnabled = true
final layer = FeatureLayer(
  FeatureLayerProperties(
    url: '...',
    popupEnabled: true,  // QUAN TRỌNG
    popupTemplate: PopupTemplate(...),
  ),
);
```

### ❌ Query trả về lỗi "Exceeded transfer rate"

**Nguyên nhân:**

- Query quá nhiều dữ liệu

**Giải pháp:**

```dart
// Thêm pagination
final queryParams = Query(
  QueryProperties(
    where: '1=1',
    outFields: ['*'],
    returnGeometry: true,
    num: 100,  // Limit 100 records
    start: 0,  // Start from record 0
  ),
);
```

### ❌ Overwrite thất bại

**Lỗi:** "Schema has changed"

**Giải pháp:**

```txt
1. Kiểm tra schema (cấu trúc trường)
2. Đảm bảo tên trường, kiểu dữ liệu không đổi
3. Nếu cần thêm trường mới:
   - Publish layer mới
   - Hoặc dùng REST API để add field
```

### 📞 Cần thêm hỗ trợ?

**Tài liệu tham khảo:**

- ArcGIS REST API: <https://developers.arcgis.com/rest/>
- ArcGIS SDK for JavaScript: <https://developers.arcgis.com/javascript/>
- Community: <https://community.esri.com/>

---

**Cập nhật lần cuối**: 21/10/2025
**Phiên bản**: 1.0
**Tác giả**: Ho Doan
