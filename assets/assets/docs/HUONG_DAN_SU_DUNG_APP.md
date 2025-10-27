# 📖 HƯỚNG DẪN SỬ DỤNG ỨNG DỤNG

## 🎯 Giới thiệu

Ứng dụng **WebGIS cho Xã** giúp bạn:

- 📍 Xem bản đồ các lớp dữ liệu địa lý
- 🔍 Tìm kiếm địa điểm
- 🗺️ Tìm đường giữa 2 điểm
- 🎯 Lọc dữ liệu theo vùng
- 📊 Xem thống kê dữ liệu
- 📚 Đọc tài liệu hướng dẫn

---

## 🚀 BẮT ĐẦU NHANH

### 1. Giao diện chính

Ứng dụng có 2 phần chính:

```txt
┌─────────────────────────────────────────────┐
│  🔍 Tìm kiếm  |  📍 Chọn vùng              │  ← Thanh công cụ
├─────────────────────────────────────────────┤
│                                       │ Tab │
│         Bản đồ                        │ Menu│
│                                       │  │  │
│                                       │  │  │
└─────────────────────────────────────────────┘
```

**Thanh công cụ (Trên cùng):**

- 🔍 Tìm kiếm địa điểm
- 📍 Chọn Tỉnh/Thành phố
- 📍 Chọn Quận/Huyện/Xã

**Menu dọc (Bên phải):**

- Lớp bản đồ
- Tìm kiếm
- Tìm đường
- Lọc
- Hướng dẫn
- Thống kê
- Tài liệu

---

## 📚 HƯỚNG DẪN TỪNG TÍNH NĂNG

### 🗺️ 1. Lớp bản đồ

**Chức năng:** Bật/tắt các lớp dữ liệu hiển thị trên bản đồ.

**Cách sử dụng:**

1. Click tab **"Lớp bản đồ"** (Tab đầu tiên)
2. Danh sách các lớp sẽ hiển thị:
   - ✅ **Checkbox**: Bật/tắt lớp
   - 👁️ **Icon mắt**: Hiển thị/Ẩn
   - 🎨 **Thanh độ mờ**: Điều chỉnh độ trong suốt

**Ví dụ:**

```txt
☑ Trường học        [████████░░] 80%
☐ Trạm y tế         [██████████] 100%
☑ UBND xã           [████████░░] 80%
```

**Mẹo:**

- Bật nhiều lớp để so sánh dữ liệu
- Giảm độ mờ để xem lớp phía dưới
- Click vào feature trên bản đồ để xem thông tin chi tiết

---

### 🔍 2. Tìm kiếm

**Chức năng:** Tìm kiếm địa điểm trên bản đồ.

**Cách sử dụng:**

#### A. Tìm kiếm nhanh (Thanh trên)

1. Click vào ô **"Phải gõ ít nhất 3 kí tự..."**
2. Gõ tên địa điểm (VD: "Trường THCS")
3. Chọn kết quả từ danh sách
4. Bản đồ tự động zoom đến địa điểm

#### B. Tìm kiếm nâng cao (Tab menu)

1. Click tab **"Tìm kiếm"**
2. Nhập từ khóa
3. Chọn loại địa điểm (nếu có)
4. Click **"Tìm kiếm"**
5. Xem danh sách kết quả
6. Click vào kết quả để xem trên bản đồ

**Mẹo:**

- Không cần gõ đầy đủ, gõ một phần là được
- Có thể gõ không dấu
- Kết quả tự động lọc theo vùng đã chọn

---

### 🗺️ 3. Tìm đường

**Chức năng:** Tìm đường đi giữa 2 điểm.

**Cách sử dụng:**

1. Click tab **"Tìm đường"**
2. Chọn **điểm xuất phát**:
   - Gõ tên địa điểm
   - Hoặc click vào bản đồ
3. Chọn **điểm đến**:
   - Gõ tên địa điểm
   - Hoặc click vào bản đồ
4. Click **"Tìm đường"**
5. Xem tuyến đường hiển thị:
   - 📏 Khoảng cách
   - ⏱️ Thời gian ước tính
   - 🗺️ Hướng dẫn từng bước

**Ví dụ:**

```txt
Từ: Trường THCS Tân Phú
Đến: Trạm y tế xã
───────────────────────
Khoảng cách: 2.5 km
Thời gian: ~10 phút
```

**Mẹo:**

- Có thể đảo ngược điểm đầu/cuối
- Click "Xóa" để tìm lại tuyến đường mới

---

### 🎯 4. Lọc dữ liệu

**Chức năng:** Lọc và hiển thị dữ liệu theo điều kiện.

**Cách sử dụng:**

1. Click tab **"Lọc"**
2. Chọn lớp dữ liệu cần lọc
3. Thiết lập điều kiện:
   - Theo vùng (Tỉnh/Quận/Xã)
   - Theo loại
   - Theo giá trị
4. Click **"Áp dụng"**
5. Xem kết quả được highlight trên bản đồ

**Ví dụ lọc:**

```txt
Lớp: Trường học
Điều kiện:
  ☑ Loại: Trung học cơ sở
  ☑ Trạng thái: Đang hoạt động
  ☑ Xã: Tân Phú
───────────────────────
Kết quả: 5 trường học
```

**Mẹo:**

- Có thể kết hợp nhiều điều kiện
- Click "Xóa bộ lọc" để xem lại tất cả

---

### 📊 5. Thống kê

**Chức năng:** Xem số liệu thống kê tổng quan.

**Cách sử dụng:**

1. Click tab **"Thống kê"**
2. Xem các chỉ số:
   - 📍 Tổng số điểm/đối tượng
   - 🗺️ Số lớp dữ liệu
   - 📏 Tổng diện tích (nếu có)
   - 📊 Phân loại theo category
3. Click **"Làm mới"** để cập nhật

**Ví dụ thống kê:**

```txt
Tổng số điểm:     156
Số lớp dữ liệu:   12
Tổng diện tích:   45.2 km²

Phân loại:
  Giáo dục:  45 (29%)  ████████
  Y tế:      23 (15%)  ████
  Hành chính: 18 (12%) ███
  Khác:      70 (44%)  ██████████
```

---

### 📚 6. Tài liệu

**Chức năng:** Xem tài liệu hướng dẫn chi tiết.

**Cách sử dụng:**

1. Click tab **"Tài liệu"**
2. Chọn tài liệu cần đọc:
   - 🚀 Bắt đầu ở đây
   - 📖 Tổng quan hệ thống
   - 💻 Hướng dẫn Code & Menu
   - 🗺️ Chuyển Layer ArcGIS
   - ✏️ Chỉnh sửa Layer
   - 📊 Tổng kết dự án
3. Đọc nội dung chi tiết
4. Click link để chuyển sang tài liệu khác

---

## 🎨 THAO TÁC TRÊN BẢN ĐỒ

### Di chuyển bản đồ

- **Kéo thả**: Click giữ chuột + di chuyển
- **Zoom in**: Scroll lên hoặc double-click
- **Zoom out**: Scroll xuống
- **Xoay**: Ctrl + Kéo chuột

### Xem thông tin feature

1. Click vào điểm/vùng trên bản đồ
2. Pop-up hiển thị thông tin:
   - Tên
   - Loại
   - Mô tả
   - Các thuộc tính khác
3. Click **"X"** để đóng pop-up

### Đo đạc

1. Tìm công cụ đo đạc (góc dưới trái)
2. Chọn loại đo:
   - 📏 Đo khoảng cách
   - 📐 Đo diện tích
3. Click các điểm trên bản đồ
4. Double-click để kết thúc
5. Xem kết quả đo

---

## 📍 LỌC THEO VÙNG

Sử dụng thanh công cụ trên cùng:

### Bước 1: Chọn Tỉnh/Thành phố

```txt
[Dropdown: Chọn thành phố ▼]
  ↓ Click
  • Hà Nội
  • Hồ Chí Minh
  • Đà Nẵng
  • ...
```

### Bước 2: Chọn Quận/Huyện/Xã

```txt
[Dropdown: Chọn Quận/Huyện ▼]
  ↓ Click (sau khi chọn thành phố)
  • Quận 1
  • Quận 2
  • Xã Tân Phú
  • ...
```

### Kết quả

- Bản đồ tự động zoom về vùng đã chọn
- Dữ liệu tự động lọc theo vùng
- Tìm kiếm chỉ trong vùng đã chọn

---

## 💡 MẸO VÀ THỦ THUẬT

### 1. Tìm kiếm nhanh

**Thay vì:**

```txt
Tab Tìm kiếm → Nhập → Click tìm → Chọn kết quả
```

**Làm nhanh hơn:**

```txt
Ô tìm kiếm trên → Gõ 3 ký tự → Chọn ngay
```

### 2. Xem nhiều lớp cùng lúc

```txt
☑ Lớp A [░░░░░░░░░░] 50%
☑ Lớp B [███████░░░] 70%
☑ Lớp C [████████░░] 80%
```

Giảm độ mờ để xem chồng lớp

### 3. So sánh dữ liệu

1. Bật lớp A
2. Click feature → Xem thông tin
3. Bật thêm lớp B
4. So sánh vị trí

### 4. Sao chép thông tin

1. Click feature
2. Pop-up hiển thị
3. Select text → Ctrl+C
4. Paste vào Excel/Word

### 5. Reset về ban đầu

- **Xóa bộ lọc**: Click "Xóa" trong tab Lọc
- **Zoom về toàn cảnh**: Click icon Home trên bản đồ
- **Tắt tất cả layers**: Uncheck all trong tab Lớp bản đồ

---

## 🔧 XỬ LÝ SỰ CỐ

### Bản đồ không hiển thị

**Nguyên nhân:**

- Kết nối internet chậm
- Layer chưa được bật

**Giải pháp:**

1. Kiểm tra internet
2. Tab "Lớp bản đồ" → Bật các layer
3. Refresh trang (F5)

### Không tìm thấy địa điểm

**Nguyên nhân:**

- Sai tên
- Không có trong dữ liệu

**Giải pháp:**

1. Kiểm tra chính tả
2. Gõ tên ngắn gọn hơn
3. Thử tìm trong tab "Tìm kiếm" nâng cao
4. Kiểm tra đã chọn đúng vùng chưa

### Pop-up không mở

**Nguyên nhân:**

- Click nhầm chỗ trống
- Layer đó không có pop-up

**Giải pháp:**

1. Click chính xác vào feature (điểm/vùng)
2. Zoom in để dễ click
3. Thử layer khác

### Tìm đường lỗi

**Nguyên nhân:**

- 2 điểm quá xa
- Không có đường đi

**Giải pháp:**

1. Chọn 2 điểm gần hơn
2. Kiểm tra lại tọa độ
3. Thử đảo ngược điểm đầu/cuối

---

## ⌨️ PHÍM TẮT

| Phím | Chức năng |
|------|-----------|
| **F5** | Refresh trang |
| **Ctrl + F** | Tìm kiếm trong trang |
| **Esc** | Đóng pop-up |
| **+/-** | Zoom in/out |
| **←→↑↓** | Di chuyển bản đồ |
| **Ctrl + Z** | Undo (nếu có) |

---

## 📱 SỬ DỤNG TRÊN MOBILE

### Thao tác cảm ứng

- **Vuốt**: Di chuyển bản đồ
- **Pinch**: Zoom in/out
- **Tap**: Click/Chọn
- **Double tap**: Zoom in nhanh
- **Two-finger drag**: Xoay bản đồ

### Lưu ý

- Menu nằm ở dạng icons nhỏ
- Một số tính năng có thể khác trên mobile
- Khuyến nghị dùng ở chế độ landscape (ngang)

---

## 📞 HỖ TRỢ

### Cần giúp đỡ?

1. **Đọc tài liệu chi tiết**
   - Tab "Tài liệu" → Chọn tài liệu phù hợp

2. **Tìm trong FAQ**
   - Tài liệu "Tổng quan hệ thống" → Phần FAQ

3. **Liên hệ hỗ trợ**
   - Email: [your-support-email]
   - Hotline: [your-phone]

---

## 🎯 CHECKLIST NGƯỜI DÙNG MỚI

```txt
☐ Đã hiểu giao diện chính
☐ Đã biết cách bật/tắt layers
☐ Đã thử tìm kiếm địa điểm
☐ Đã biết cách xem thông tin feature
☐ Đã thử lọc dữ liệu theo vùng
☐ Đã xem thống kê
☐ Đã biết cách tìm đường
☐ Đã đọc tài liệu chi tiết
```

---

## 🚀 BẮT ĐẦU NGAY

**Thử ngay 3 bước:**

1. **Chọn vùng** → Dropdown → Chọn thành phố của bạn
2. **Bật layer** → Tab "Lớp bản đồ" → Bật vài lớp
3. **Click vào bản đồ** → Xem thông tin chi tiết

🎉 **Chúc bạn sử dụng hiệu quả!**

---

**Cập nhật**: 21/10/2025  
**Phiên bản**: 1.0  
**Tác giả**: Ho Doan
