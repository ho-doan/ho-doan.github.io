# HƯỚNG DẪN ĐƠN GIẢN CHO CÁN BỘ XÃ
## Thêm dữ liệu mới lên Web từ Excel/CAD

> **Mục đích:** Giúp cán bộ xã (người không chuyên IT) có thể tự thêm dữ liệu mới lên hệ thống web mà không cần hỗ trợ kỹ thuật.

---

## 📋 CHUẨN BỊ

### Bạn cần có:
- ✅ **Máy tính** có cài ArcGIS Pro (hoặc nhờ người có)
- ✅ **File Excel** chứa dữ liệu (VD: danh sách nhà dân, đường xá)
- ✅ **Tài khoản ArcGIS Online** (được cấp bởi quản trị viên)
- ✅ **Kết nối internet**

### Thời gian dự kiến:
- **Lần đầu:** 1-2 tiếng (học cách làm)
- **Lần sau:** 15-30 phút (đã quen)

---

## 🔢 QUY TRÌNH 5 BƯỚC ĐƠN GIẢN

```txt
┌──────────────┐
│ 1. Excel     │ → Chuẩn bị dữ liệu trong Excel
└──────┬───────┘
       ▼
┌──────────────┐
│ 2. ArcGIS Pro│ → Mở Excel trong ArcGIS Pro
└──────┬───────┘
       ▼
┌──────────────┐
│ 3. Đặt vị trí│ → Đánh dấu vị trí trên bản đồ
└──────┬───────┘
       ▼
┌──────────────┐
│ 4. Publish   │ → Đẩy lên ArcGIS Online
└──────┬───────┘
       ▼
┌──────────────┐
│ 5. Web       │ → Kiểm tra trên web
└──────────────┘
```

---

## BƯỚC 1: CHUẨN BỊ FILE EXCEL

### Ví dụ: Danh sách nhà dân cần thêm

Mở Excel và tạo bảng như sau:

| STT | Ho_Ten         | Dia_Chi          | Kinh_Do   | Vi_Do     | Dien_Thoai   | Loai_Nha |
|-----|----------------|------------------|-----------|-----------|--------------|----------|
| 1   | Nguyễn Văn A   | Thôn 1, xã X     | 106.12345 | 10.23456  | 0987654321   | Cấp 4    |
| 2   | Trần Thị B     | Thôn 2, xã X     | 106.12356 | 10.23467  | 0987654322   | Kiên cố  |
| 3   | Lê Văn C       | Thôn 3, xã X     | 106.12367 | 10.23478  | 0987654323   | Tạm      |

### ⚠️ LƯU Ý QUAN TRỌNG:

1. **Hàng đầu tiên** là tên cột (không dấu, không khoảng trắng)
   - ✅ Tốt: `Ho_Ten`, `Dien_Thoai`, `Loai_Nha`
   - ❌ Tệ: `Họ tên`, `Điện thoại`, `Loại nhà`

2. **Phải có cột tọa độ:**
   - `Kinh_Do` (Longitude) = Kinh độ (VD: 106.12345)
   - `Vi_Do` (Latitude) = Vĩ độ (VD: 10.23456)
   
3. **Cách lấy tọa độ:**
   - **Cách 1 - Google Maps:**
     1. Vào https://www.google.com/maps
     2. Click chuột phải vào vị trí nhà
     3. Chọn "Sao chép tọa độ GPS"
     4. Paste vào Excel (VD: `10.23456, 106.12345`)
     5. Tách ra 2 cột: `Vi_Do` và `Kinh_Do`
   
   - **Cách 2 - Điện thoại GPS:**
     1. Dùng app "GPS Status" (Android) hoặc "Compass" (iOS)
     2. Đi đến vị trí nhà
     3. Ghi lại tọa độ hiển thị
     4. Nhập vào Excel

4. **Lưu file:**
   - Lưu dạng **Excel (.xlsx)** hoặc **CSV (.csv)**
   - Đặt tên rõ ràng: `NhaDan_Xa_X_2024.xlsx`

---

## BƯỚC 2: MỞ EXCEL TRONG ARCGIS PRO

### 2.1. Mở ArcGIS Pro
1. **Khởi động ArcGIS Pro** (icon màu xanh trên desktop)
2. **Chọn "New Map"** (bản đồ mới)
3. **Đợi map load xong** (có lưới đường và nền)

### 2.2. Import Excel vào ArcGIS Pro
1. **Vào menu "Map" > "Add Data" > "XY Point Data"**
2. **Chọn file Excel** vừa tạo
3. **Cấu hình:**
   - **X Field (Longitude):** Chọn cột `Kinh_Do`
   - **Y Field (Latitude):** Chọn cột `Vi_Do`
   - **Coordinate System:** Chọn `WGS 1984` (hoặc `GCS_WGS_1984`)
4. **Nhấn "OK"**

### 2.3. Kiểm tra
- ✅ Các điểm xuất hiện trên bản đồ
- ✅ Vị trí đúng (so với đường, sông, ranh giới)
- ❌ Nếu sai: Kiểm tra lại tọa độ trong Excel

---

## BƯỚC 3: TẠO FEATURE LAYER

### 3.1. Chuyển từ điểm tạm sang layer cố định
1. **Click chuột phải vào layer vừa thêm** (trong Table of Contents bên trái)
2. **Chọn "Data" > "Export Features"**
3. **Đặt tên output:** `NhaDan_Xa_X`
4. **Chọn vị trí lưu:** Project Geodatabase (mặc định)
5. **Nhấn "Run"**
6. **Xóa layer tạm** (layer có icon bảng Excel) - không cần nữa

### 3.2. Tùy chỉnh hiển thị (tùy chọn)

#### A. Thay đổi màu sắc/biểu tượng
1. **Click chuột phải vào layer** > **Symbology**
2. **Chọn biểu tượng:**
   - Nhà: Chọn icon ngôi nhà
   - Trường: Chọn icon trường học
   - Bệnh viện: Chọn icon dấu cộng
3. **Chọn màu:** Đỏ, xanh, vàng, ...
4. **Điều chỉnh kích thước:** 8-12 pt (vừa phải)

#### B. Tạo nhãn tự động
1. **Click chuột phải vào layer** > **Label**
2. **Bật "Label Features"**
3. **Chọn field hiển thị:** Chọn `Ho_Ten` (tên chủ hộ)
4. **Điều chỉnh font:** Arial, size 10

---

## BƯỚC 4: PUBLISH LÊN ARCGIS ONLINE

### 4.1. Đăng nhập ArcGIS Online
1. **Vào menu "Project" > "Sign In"**
2. **Nhập:**
   - Username: `canboxaphuongduc`
   - Password: `********` (được cấp)
3. **Nhấn "Sign In"**

### 4.2. Publish Web Layer
1. **Click chuột phải vào layer** `NhaDan_Xa_X`
2. **Chọn "Sharing" > "Share As Web Layer"**
3. **Cấu hình publish:**

#### A. General (Trang 1)
- **Name:** `NhaDan_Xa_X_2024` (không dấu, không khoảng trắng)
- **Summary:** "Danh sách nhà dân xã X năm 2024"
- **Tags:** `nhadan`, `xaX`, `2024` (phân cách bằng dấu phẩy)
- **Share with:** Chọn `Everyone` (công khai) hoặc `Organization` (nội bộ)

#### B. Configuration (Trang 2)
- **Layer Type:** `Feature`
- **Enable editing:** ✅ **Check** (cho phép sửa sau này)
- **Enable Sync:** ✅ **Check** (cho phép offline)

#### C. Content (Trang 3)
- Giữ nguyên mặc định

4. **Nhấn "Analyze"** (kiểm tra lỗi)
   - ✅ Nếu không lỗi: Nhấn "Publish"
   - ❌ Nếu có lỗi: Sửa theo hướng dẫn, rồi "Analyze" lại

5. **Đợi publish xong** (1-5 phút, tùy số lượng điểm)
   - Thanh tiến trình chạy
   - Khi xong sẽ có thông báo "Successfully published"

### 4.3. Copy URL của layer
1. **Nhấn "Manage the web layer"** (sau khi publish xong)
2. **Copy URL** (dạng: `https://services.arcgis.com/.../FeatureServer`)
3. **Lưu lại** để dùng ở bước 5

---

## BƯỚC 5: CẬP NHẬT VÀO WEB APP

### 5.1. Thêm layer vào code (cần hỗ trợ lập trình viên)

> ⚠️ **Phần này cần người lập trình thực hiện.** Hãy gửi thông tin sau cho lập trình viên:

**Thông tin cần gửi:**
- ✅ **Tên layer:** `NhaDan_Xa_X_2024`
- ✅ **URL:** `https://services.arcgis.com/.../FeatureServer`
- ✅ **Layer ID:** (thường là 0, 1, 2, ...)
- ✅ **Mô tả:** "Danh sách nhà dân xã X năm 2024"

**Ví dụ email gửi:**
```
Chào anh [Lập trình viên],

Em đã publish layer mới lên ArcGIS Online. Anh giúp em thêm vào web app với thông tin sau:

- Tên: NhaDan_Xa_X_2024
- URL: https://services.arcgis.com/xxx/arcgis/rest/services/NhaDan_Xa_X_2024/FeatureServer
- Layer ID: 0
- Mô tả: Danh sách nhà dân xã X năm 2024

Cảm ơn anh!
```

### 5.2. Kiểm tra trên web (sau khi lập trình viên thêm xong)

1. **Mở trình duyệt** (Chrome, Firefox, Safari)
2. **Truy cập:** `https://your-web-app.com/#/home`
3. **Kiểm tra:**
   - ✅ Layer mới xuất hiện trong danh sách bên trái
   - ✅ Bật layer, các điểm hiển thị đúng vị trí
   - ✅ Click vào điểm, thông tin hiển thị đầy đủ
   - ✅ Vào tab "Thống kê", thử thống kê layer mới

---

## 🔄 CẬP NHẬT DỮ LIỆU (OVERWRITE)

### Khi nào cần cập nhật?
- Thêm nhà mới
- Sửa thông tin nhà cũ
- Xóa nhà không còn

### Cách cập nhật:

#### A. Cập nhật file Excel
1. **Mở file Excel gốc**
2. **Thêm/Sửa/Xóa dữ liệu**
3. **Lưu file**

#### B. Update trong ArcGIS Pro
1. **Xóa layer cũ** trong ArcGIS Pro
2. **Import lại Excel** (Bước 2.2)
3. **Export Features** (Bước 3.1)
4. **QUAN TRỌNG:** Đặt **ĐÚNG TÊN** như layer cũ
   - VD: Cũ là `NhaDan_Xa_X`, mới cũng phải là `NhaDan_Xa_X`

#### C. Publish lại (Overwrite)
1. **Click chuột phải vào layer** > **Sharing** > **Overwrite Web Layer**
2. **Chọn layer cũ** trong dropdown
3. **Nhấn "Analyze"** > **"Overwrite"**
4. **Đợi xong**

#### D. Kiểm tra
- Vào web, **refresh trang** (F5)
- Dữ liệu mới xuất hiện ngay

---

## ❓ CÂU HỎI THƯỜNG GẶP (FAQ)

### Q1: Tôi không có ArcGIS Pro, làm sao?
**Trả lời:**
- **Cách 1:** Dùng máy của đồng nghiệp có cài
- **Cách 2:** Liên hệ phòng Tài nguyên Môi trường huyện
- **Cách 3:** Gửi file Excel cho quản trị viên, nhờ họ publish giúp

### Q2: Tôi không biết tọa độ của các điểm, làm sao?
**Trả lời:**
- **Cách 1:** Dùng Google Maps (xem Bước 1.3)
- **Cách 2:** Dùng điện thoại có GPS (app GPS Status)
- **Cách 3:** Nhờ lập trình viên tạo form nhập liệu trên web

### Q3: Publish bị lỗi, phải làm gì?
**Trả lời:**
- **Xem thông báo lỗi** trong cửa sổ Analyze
- **Lỗi thường gặp:**
  - Tên layer có dấu → Xóa dấu
  - Field name quá dài → Rút ngắn
  - Không có quyền publish → Liên hệ admin cấp quyền

### Q4: Web không hiển thị layer mới?
**Trả lời:**
- **Bước 1:** Refresh trang (Ctrl+R hoặc F5)
- **Bước 2:** Xóa cache trình duyệt
- **Bước 3:** Kiểm tra lập trình viên đã thêm vào code chưa

### Q5: Tôi muốn sửa dữ liệu sau khi publish?
**Trả lời:**
- **Cách 1:** Overwrite (xem phần "Cập nhật dữ liệu")
- **Cách 2:** Sửa trực tiếp trên ArcGIS Online:
  1. Vào https://www.arcgis.com
  2. Đăng nhập
  3. Vào "Content" > Tìm layer
  4. Chọn "Open in Map Viewer"
  5. Edit trực tiếp

---

## 📞 LIÊN HỆ HỖ TRỢ

**Khi gặp khó khăn, liên hệ:**

### Hỗ trợ kỹ thuật
- **Họ tên:** Nguyễn Văn B (Lập trình viên)
- **Điện thoại:** 0912345678
- **Email:** dev@example.com

### Quản trị viên hệ thống
- **Họ tên:** Trần Thị C
- **Điện thoại:** 0987654321
- **Email:** admin@example.com

---

## ✅ CHECKLIST

Sau khi hoàn thành, bạn có thể:

- [ ] Chuẩn bị file Excel đúng định dạng
- [ ] Lấy tọa độ GPS cho các điểm
- [ ] Mở Excel trong ArcGIS Pro
- [ ] Tạo Feature Layer
- [ ] Tùy chỉnh symbology
- [ ] Publish lên ArcGIS Online
- [ ] Kiểm tra trên web
- [ ] Cập nhật dữ liệu (Overwrite)

**🎉 Chúc mừng! Bạn đã có thể tự quản lý dữ liệu!**

