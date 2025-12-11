# HƯỚNG DẪN KẾT NỐI ĐIỆN THOẠI VÀ ĐỒNG BỘ DỮ LIỆU

## MỤC LỤC

1. [Tổng quan](#1-tổng-quan)
2. [Chuẩn bị](#2-chuẩn-bị)
3. [Cài đặt ArcGIS Field Maps](#3-cài-đặt-arcgis-field-maps)
4. [Cài đặt ArcGIS Survey123](#4-cài-đặt-arcgis-survey123)
5. [Thu thập dữ liệu trên điện thoại](#5-thu-thập-dữ-liệu-trên-điện-thoại)
6. [Đồng bộ dữ liệu lên Web](#6-đồng-bộ-dữ-liệu-lên-web)
7. [Xử lý sự cố](#7-xử-lý-sự-cố)

---

## 1. TỔNG QUAN

### 1.1. Quy trình làm việc

#### Bước 1: Thu thập dữ liệu ngoài thực địa

- **Điện thoại (Field Maps)**
  - Xem bản đồ offline
  - Thêm/sửa điểm, đường, vùng
  - Chụp ảnh đính kèm
  - Ghi chú thông tin

#### Bước 2: Đồng bộ khi có WiFi/4G

- **ArcGIS Online (Server)**
  - Lưu trữ dữ liệu
  - Quản lý phiên bản
  - Phân quyền truy cập

#### Bước 3: Tự động cập nhật

- **Web App (Flutter)**
  - Hiển thị dữ liệu mới
  - Thống kê tự động
  - Báo cáo real-time

### 1.2. Thời gian dự kiến

- **Cài đặt lần đầu:** 30-45 phút
- **Thu thập 1 điểm:** 2-3 phút
- **Đồng bộ 100 điểm:** 5-10 phút (tùy mạng)

---

## 2. CHUẨN BỊ

### 2.1. Yêu cầu thiết bị

**Điện thoại:**

- Android 8.0+ hoặc iOS 13+
- Có GPS (định vị)
- Camera (chụp ảnh)
- Dung lượng trống: tối thiểu 2GB

**Kết nối:**

- WiFi hoặc 3G/4G để đồng bộ
- Có thể làm việc OFFLINE (đồng bộ sau)

### 2.2. Tài khoản cần thiết

#### 1. ArcGIS Online Account

- Email: <canboxaphuongduc@example.com>
- Password: (được cấp bởi quản trị viên)
- Role: **Field Worker** hoặc **Editor**

#### 2. Quyền truy cập

- Xem và chỉnh sửa Feature Layer
- Tải bản đồ offline
- Upload ảnh đính kèm

---

## 3. CÀI ĐẶT ARCGIS FIELD MAPS

### 3.1. Bước 1: Tải ứng dụng

#### Android

1. Mở **Google Play Store**
2. Tìm kiếm: **"ArcGIS Field Maps"**
3. Nhấn **"Cài đặt"**
4. Đợi tải về (khoảng 200MB)

#### iOS

1. Mở **App Store**
2. Tìm kiếm: **"ArcGIS Field Maps"**
3. Nhấn **"Tải"**
4. Xác thực Face ID/Touch ID

### 3.2. Bước 2: Đăng nhập

1. Mở app **Field Maps**
2. Chọn **"Sign In"**
3. Nhập thông tin:
   - Organization URL: <https://www.arcgis.com>
   - Username: canboxaphuongduc
   - Password: (nhập mật khẩu)
4. Nhấn **"Sign In"**

### 3.3. Bước 3: Tải bản đồ để làm việc offline

1. Vào tab **"Maps"**
2. Tìm map: **"Bản đồ xã Phương Đức"**
3. Nhấn vào map để xem chi tiết
4. Nhấn icon **"Download"** (mũi tên xuống)
5. Chọn vùng cần tải:
   - Zoom vào khu vực xã của bạn
   - Khung màu xanh = vùng sẽ tải
   - Giảm vùng nếu dung lượng quá lớn
6. Chọn **"Download"**
7. Đợi tải xong (5-15 phút, tùy vùng)

> **LƯU Ý:** Cần WiFi để tải offline map (dung lượng lớn)

### 3.4. Bước 4: Bật GPS và Camera

1. Vào **Settings** điện thoại
2. **Location/Vị trí:**
   - Bật "Location Services"
   - Cho phép Field Maps truy cập vị trí
   - Chọn "While Using the App" hoặc "Always"
3. **Camera:**
   - Cho phép Field Maps truy cập camera
4. **Storage/Bộ nhớ:**
   - Cho phép lưu trữ

---

## 4. CÀI ĐẶT ARCGIS SURVEY123

> **LƯU Ý:** Survey123 phù hợp cho **thu thập dữ liệu có form cố định** (VD: Khảo sát dân số, điều tra đất đai)

### 4.1. Bước 1: Tải ứng dụng

- **Android:** Google Play Store → "ArcGIS Survey123"
- **iOS:** App Store → "ArcGIS Survey123"

### 4.2. Bước 2: Đăng nhập

Đăng nhập tương tự như Field Maps (xem mục 3.2)

### 4.3. Bước 3: Tải survey form

1. Mở app Survey123
2. Nhấn **"Download Surveys"**
3. Chọn survey: **"Điều tra hộ gia đình xã Phương Đức"**
4. Nhấn **"Download"**

### 4.4. Bước 4: Làm offline

1. Vào **Settings survey**
2. Bật **"Allow offline surveys"**
3. Tải basemap nếu cần

---

## 5. THU THẬP DỮ LIỆU TRÊN ĐIỆN THOẠI

### 5.1. A. Thêm điểm mới (VD: Nhà dân, trụ điện)

#### Bước 1: Mở map và tìm layer

1. Mở **Field Maps**
2. Chọn **map đã tải offline**
3. Nhấn **icon "+" (Add)**
4. Chọn **layer:** "Nhà dân" hoặc layer cần thêm

#### Bước 2: Đặt vị trí

**Cách 1 - GPS tự động:**

1. Đến vị trí thực tế
2. Nhấn **"Capture Location"** (icon GPS)
3. Đợi GPS chính xác (độ chính xác < 5m)

**Cách 2 - Chọn trên bản đồ:**

1. Zoom vào khu vực
2. Nhấn vào vị trí trên map
3. Điều chỉnh nếu cần

#### Bước 3: Điền thông tin

1. **Họ tên chủ hộ:** Nguyễn Văn A
2. **Số điện thoại:** 0987654321
3. **Loại nhà:** Chọn "Nhà cấp 4"
4. **Diện tích:** 120 (m²)
5. **Ghi chú:** "Nhà gần chợ"

#### Bước 4: Chụp ảnh

1. Nhấn **"Add Attachment"** (icon camera)
2. Chọn **"Take Photo"** hoặc **"Choose from Library"**
3. Chụp ảnh nhà
4. Thêm nhiều ảnh nếu cần

#### Bước 5: Lưu

1. Nhấn **"Submit"**
2. Dữ liệu được lưu **locally** (chưa đồng bộ)

### 5.2. B. Vẽ đường (VD: Đường giao thông, sông suối)

#### Bước 1-2: Như thêm điểm

Thực hiện tương tự như Bước 1-2 của phần thêm điểm mới.

#### Bước 3: Vẽ đường

1. Chọn **line tool**
2. Nhấn dọc theo đường:
   - Mỗi lần nhấn = 1 điểm
   - GPS tự động ghi tọa độ
   - Có thể đi bộ và nhấn từng đoạn
3. Kết thúc: Nhấn 2 lần vào điểm cuối

#### Bước 4: Điền thông tin đường

- Tên đường: "Đường Trần Hưng Đạo"
- Loại mặt đường: Chọn "Đường đất" / "Bê tông" / "Nhựa"
- Chiều rộng: 3.5 (m)
- Tình trạng: Chọn "Tốt" / "Xuống cấp"

#### Bước 5: Lưu đường

Nhấn **"Submit"** để lưu đường đã vẽ.

### 5.3. C. Vẽ vùng (VD: Ao hồ, thửa đất)

Tương tự vẽ đường, nhưng:

- Vẽ khép kín (điểm đầu = điểm cuối)
- Diện tích tự động tính

### 5.4. D. Sửa dữ liệu đã có

1. Nhấn vào feature trên map
2. Chọn **"Edit"**
3. Sửa thông tin hoặc di chuyển vị trí
4. Nhấn **"Submit"**

### 5.5. E. Xóa dữ liệu

1. Nhấn vào feature
2. Chọn **"Delete"**
3. Xác nhận **"Yes"**

---

## 6. ĐỒNG BỘ DỮ LIỆU LÊN WEB

### 6.1. Cách 1: Đồng bộ thủ công

#### Bước 1: Kết nối mạng

- Bật WiFi hoặc 4G
- Đảm bảo tín hiệu ổn định

#### Bước 2: Đồng bộ

1. Mở **Field Maps**
2. Nhấn **icon "Sync"** (mũi tên tròn) ở góc trên
3. Chọn **"Sync Now"**
4. Đợi đồng bộ hoàn tất (thanh tiến trình)

#### Bước 3: Kiểm tra

- **Màu xanh:** Đồng bộ thành công
- **Màu đỏ:** Có lỗi (xem phần xử lý sự cố)
- **Màu xanh dương:** Chưa đồng bộ (Pending)

### 6.2. Cách 2: Tự động đồng bộ

#### Cài đặt tự động sync

1. Vào **Settings > Sync**
2. Bật **"Auto Sync"**
3. Chọn tần suất:
   - Every 5 minutes
   - Every 15 minutes
   - When WiFi available

> **LƯU Ý:** Auto sync tốn pin và dữ liệu 3G/4G

### 6.3. Kiểm tra trên Web

#### Sau khi đồng bộ xong

1. Mở trình duyệt (Chrome/Safari)
2. Truy cập: <https://your-web-app.com/#/home>
3. Xem map:
   - Dữ liệu mới xuất hiện **ngay lập tức**
   - Click vào feature để xem thông tin
   - Ảnh đã upload hiển thị
4. Vào tab **"Thống kê":**
   - Số liệu tự động cập nhật
   - Biểu đồ real-time

---

## 7. XỬ LÝ SỰ CỐ

### 7.1. Lỗi 1: Không đồng bộ được

**Triệu chứng:**

- Icon sync màu đỏ
- Message: "Sync failed"

**Nguyên nhân & Giải pháp:**

#### A. Không có mạng

- **Kiểm tra:** Bật WiFi hoặc 4G
- **Test:** Mở trình duyệt xem web được không

#### B. Tài khoản hết hạn hoặc thiếu quyền

- **Kiểm tra:** Đăng xuất và đăng nhập lại
- **Liên hệ:** Quản trị viên kiểm tra quyền

#### C. Dữ liệu xung đột (conflict)

- **Nguyên nhân:** 2 người sửa cùng 1 feature
- **Giải pháp:**
  1. Field Maps sẽ hiển thị cảnh báo
  2. Chọn **"Keep yours"** (giữ của bạn) hoặc **"Keep theirs"** (giữ của người khác)
  3. Sync lại

#### D. File ảnh quá lớn

- **Giải pháp:**
  1. Vào Settings > Camera
  2. Giảm "Photo Quality" xuống "Medium" hoặc "Low"
  3. Thử sync lại

### 7.2. Lỗi 2: GPS không chính xác

**Triệu chứng:**

- Vị trí nhảy lung tung
- Độ chính xác > 10m

**Giải pháp:**

1. Ra ngoài trời (tránh trong nhà, gầm cây)
2. Đợi 30-60 giây để GPS ổn định
3. Bật "High Accuracy" trong Settings điện thoại
4. Khởi động lại GPS:
   - Bật/tắt "Airplane Mode"
   - Hoặc tắt/bật "Location Services"

### 7.3. Lỗi 3: Không tải được offline map

**Nguyên nhân:**

- Dung lượng không đủ
- Vùng tải quá lớn
- Layer không hỗ trợ offline

**Giải pháp:**

1. **Giải phóng dung lượng:**
   - Xóa app không dùng
   - Xóa ảnh/video cũ
   - Cần tối thiểu 2GB trống
2. **Giảm vùng tải:**
   - Zoom vào khu vực nhỏ hơn
   - Chỉ tải khu vực xã, không tải cả huyện
3. **Giảm chi tiết:**
   - Chọn "Download lower detail"
   - Bỏ một số layer không cần thiết

### 7.4. Lỗi 4: App bị crash / treo

**Giải pháp:**

1. **Force close app:**
   - Android: Settings > Apps > Field Maps > Force Stop
   - iOS: Vuốt lên từ dưới, vuốt app ra ngoài
2. **Xóa cache:**
   - Android: Settings > Apps > Field Maps > Clear Cache
   - iOS: Xóa và cài lại app (dữ liệu chưa sync sẽ mất!)
3. **Cập nhật app:**
   - Vào Play Store/App Store
   - Kiểm tra phiên bản mới
4. **Khởi động lại điện thoại**

### 7.5. Lỗi 5: Dữ liệu bị mất sau khi đồng bộ

**Nguyên nhân:**

- Lỗi server
- Xung đột dữ liệu
- Xóa nhầm

**Giải pháp:**

1. **Kiểm tra trên web:**
   - Có thể chỉ không hiển thị trên app
   - Đăng nhập web để kiểm tra
2. **Restore từ backup:**
   - Liên hệ quản trị viên
   - ArcGIS Online có lưu version history
3. **Nhập lại:**
   - Nếu không restore được
   - Phải nhập lại dữ liệu

---

## 8. WORKFLOW HÀNG NGÀY

### 8.1. Sáng (Chuẩn bị đi thực địa)

1. Sạc đầy pin điện thoại
2. Mở Field Maps, kiểm tra map offline
3. Sync một lần để có dữ liệu mới nhất
4. Bật GPS
5. Mang theo:
   - Sạc dự phòng
   - Bút + giấy (backup)
   - Thước đo (nếu cần)

### 8.2. Ngoài thực địa

1. Mở map, tìm vị trí hiện tại
2. Thêm/sửa dữ liệu:
   - Đặt điểm GPS
   - Điền form
   - Chụp ảnh
   - Lưu
3. Lặp lại cho các điểm khác
4. Không cần sync ngay (làm offline)

### 8.3. Chiều (Về văn phòng)

1. Kết nối WiFi
2. Mở Field Maps
3. Nhấn **Sync**
4. Đợi đồng bộ xong
5. Kiểm tra trên web:
   - Vào <https://your-web-app.com>
   - Xem dữ liệu mới
   - Kiểm tra ảnh
6. Báo cáo lãnh đạo (nếu cần)

---

## 9. MẸO VÀ THỦ THUẬT

### 9.1. Tiết kiệm pin

- Bật "Airplane Mode" khi đi thực địa (vẫn dùng GPS)
- Giảm độ sáng màn hình
- Tắt sync tự động
- Dùng sạc dự phòng

### 9.2. Tăng độ chính xác GPS

- Đứng yên 30s trước khi capture location
- Ra ngoài trời thoáng
- Tránh cao ốc, núi che phủ
- Dùng external GPS receiver (nếu có ngân sách)

### 9.3. Quản lý dữ liệu

- Đặt tên rõ ràng cho ghi chú
- Chụp ảnh góc rộng trước, góc hẹp sau
- Ghi ngày tháng vào ghi chú
- Sync ít nhất 1 lần/ngày

### 9.4. Làm việc nhóm

- Chia khu vực cho từng người
- Họp sáng để phân công
- Sync cùng lúc để tránh xung đột
- Dùng walkie-talkie nếu mạng yếu

---

## 10. HỖ TRỢ

### 10.1. Liên hệ khi cần trợ giúp

**Quản trị viên hệ thống:**

- Họ tên: Nguyễn Văn B
- Điện thoại: 0912345678
- Email: <admin@example.com>

**Hỗ trợ kỹ thuật ArcGIS:**

- Website: <https://support.esri.com>
- Hotline: 1900-xxxx

### 10.2. Tài liệu tham khảo

- [ArcGIS Field Maps Documentation](https://doc.arcgis.com/en/field-maps/)
- [ArcGIS Survey123 Documentation](https://doc.arcgis.com/en/survey123/)

---

## 11. CHECKLIST HOÀN THÀNH

Sau khi đọc hướng dẫn này, bạn có thể:

- [ ] Cài đặt Field Maps trên điện thoại
- [ ] Đăng nhập với tài khoản được cấp
- [ ] Tải offline map
- [ ] Thêm điểm mới với GPS
- [ ] Vẽ đường và vùng
- [ ] Chụp và đính kèm ảnh
- [ ] Đồng bộ dữ liệu lên server
- [ ] Kiểm tra dữ liệu trên web app
- [ ] Xử lý các lỗi thường gặp

**Chúc bạn làm việc hiệu quả!**
