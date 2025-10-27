# HƯỚNG DẪN CODE VÀ CHỈNH SỬA MENU

## 📋 MỤC LỤC

1. [Giới thiệu Nền tảng](#1-giới-thiệu-nền-tảng)
2. [Cấu trúc Dự án](#2-cấu-trúc-dự-án)
3. [Cách Chạy Ứng dụng](#3-cách-chạy-ứng-dụng)
4. [Hướng dẫn Chỉnh sửa Menu](#4-hướng-dẫn-chỉnh-sửa-menu)
5. [Thêm Tab Menu Mới](#5-thêm-tab-menu-mới)
6. [Tùy chỉnh Giao diện](#6-tùy-chỉnh-giao-diện)

---

## 1. GIỚI THIỆU NỀN TẢNG

### 🎯 Công nghệ sử dụng

- **Frontend Framework**: Flutter (Web)
- **Map SDK**: ArcGIS SDK for JavaScript
- **State Management**: Flutter Bloc
- **Dependency Injection**: GetIt + Injectable
- **Routing**: Go Router

### 📱 Tính năng chính

- Hiển thị bản đồ ArcGIS
- Quản lý các lớp dữ liệu (layers)
- Tìm kiếm địa điểm
- Tìm đường
- Lọc dữ liệu theo vùng
- Đo đạc (measurement)

---

## 2. CẤU TRÚC DỰ ÁN

```txt
arcgis_web/
├── arcgis_sdk/                    # Plugin ArcGIS SDK
│   ├── lib/
│   │   ├── arcgis_sdk.dart       # Main SDK file
│   │   ├── web/                   # Web implementation
│   │   └── widgets/               # Map widgets
│   └── assets/
│       └── js_api/                # ArcGIS JavaScript API files
│
└── example/                       # Ứng dụng chính
    ├── lib/
    │   └── src/
    │       ├── common/            # Widgets chung
    │       │   ├── themes/        # Màu sắc, styles
    │       │   └── widgets/       # Word ribbon
    │       ├── core/              # Cấu hình core
    │       │   ├── router/        # Routing
    │       │   └── service/       # Services, DI
    │       └── features/          # Các tính năng
    │           ├── app/           # App chính
    │           └── home/          # Trang chủ
    │               ├── home_page.dart
    │               └── widgets/
    │                   └── tabs/
    │                       ├── layer.dart      # Tab quản lý layers
    │                       ├── search.dart     # Tab tìm kiếm
    │                       ├── direction.dart  # Tab tìm đường
    │                       ├── filter/         # Tab lọc dữ liệu
    │                       └── guide/          # Tab hướng dẫn
    ├── assets/                    # Hình ảnh, icons
    └── web/
        └── index.html             # Entry point web
```

---

## 3. CÁCH CHẠY ỨNG DỤNG

### ⚙️ Yêu cầu hệ thống

- Flutter SDK >= 3.6.0
- Dart SDK >= 3.6.0
- Web browser (Chrome khuyến nghị)

### 📦 Cài đặt

```bash
# 1. Di chuyển vào thư mục example
cd arcgis_web/example

# 2. Cài đặt dependencies
flutter pub get

# 3. Chạy code generation (nếu cần)
flutter pub run build_runner build --delete-conflicting-outputs

# 4. Chạy ứng dụng trên web
flutter run -d chrome

# Hoặc build để deploy
flutter build web
```

### 🌐 Sau khi build

- File build sẽ nằm trong `example/build/web/`
- Copy toàn bộ nội dung vào web server (Apache, Nginx, hoặc Firebase Hosting)

---

## 4. HƯỚNG DẪN CHỈNH SỬA MENU

### 📍 Vị trí file menu chính

Menu hiện tại nằm trong file: `example/lib/src/features/home/home_page.dart`

### 🎨 Cấu trúc Menu hiện tại

Menu được định nghĩa ở dòng 28:

```dart
const _actions = ['Lớp bản đồ', 'Tìm kiếm', 'Tìm đường', 'Lọc', 'Hướng dẫn'];
```

### 📝 Cách chỉnh sửa tên menu

**Bước 1**: Mở file `home_page.dart`

**Bước 2**: Tìm dòng 28 và sửa tên menu:

```dart
// CŨ
const _actions = ['Lớp bản đồ', 'Tìm kiếm', 'Tìm đường', 'Lọc', 'Hướng dẫn'];

// MỚI - Thay đổi tên theo ý muốn
const _actions = ['Bản đồ', 'Tra cứu', 'Chỉ đường', 'Bộ lọc', 'Trợ giúp'];
```

**Bước 3**: Lưu file và chạy lại ứng dụng

### 🎯 Cách hiển thị menu

Menu được hiển thị ở 2 nơi:

1. **Thanh Tab bên phải** (Vertical tabs) - dòng 768-770
2. **Nội dung Tab** - dòng 698-714

---

## 5. THÊM TAB MENU MỚI

### 📋 Quy trình thêm tab mới

Ví dụ: Thêm tab "**Thống kê**"

#### **Bước 1**: Thêm tên tab vào danh sách

Trong `home_page.dart`, dòng 28:

```dart
// TRƯỚC
const _actions = ['Lớp bản đồ', 'Tìm kiếm', 'Tìm đường', 'Lọc', 'Hướng dẫn'];

// SAU
const _actions = ['Lớp bản đồ', 'Tìm kiếm', 'Tìm đường', 'Lọc', 'Hướng dẫn', 'Thống kê'];
```

#### **Bước 2**: Cập nhật TabController

Trong `_HomePageState.initState()`, dòng 52:

```dart
// TRƯỚC
_tabController = TabController(length: _actions.length, vsync: this);

// SAU - Không cần thay đổi vì đã dùng _actions.length (tự động)
_tabController = TabController(length: _actions.length, vsync: this);
```

#### **Bước 3**: Tạo file widget cho tab mới

Tạo file: `example/lib/src/features/home/widgets/tabs/statistics/statistics.dart`

```dart
import 'package:flutter/material.dart';
import 'package:arcgis_sdk/arcgis_sdk_web.dart';

class StatisticsTab extends StatefulWidget {
  const StatisticsTab({
    super.key,
    required this.controller,
  });

  final MapViewController controller;

  @override
  State<StatisticsTab> createState() => _StatisticsTabState();
}

class _StatisticsTabState extends State<StatisticsTab> {
  @override
  Widget build(BuildContext context) {
    return Container(
      color: Colors.white,
      padding: const EdgeInsets.all(16),
      child: Column(
        crossAxisAlignment: CrossAxisAlignment.start,
        children: [
          // Header
          const Text(
            'Thống kê dữ liệu',
            style: TextStyle(
              fontSize: 18,
              fontWeight: FontWeight.bold,
              color: Color(0xFF323130),
            ),
          ),
          const SizedBox(height: 16),
          
          // Nội dung thống kê
          Expanded(
            child: SingleChildScrollView(
              child: Column(
                crossAxisAlignment: CrossAxisAlignment.start,
                children: [
                  _buildStatCard(
                    title: 'Tổng số điểm',
                    value: '0',
                    icon: Icons.location_on,
                    color: Colors.blue,
                  ),
                  const SizedBox(height: 12),
                  _buildStatCard(
                    title: 'Tổng diện tích (km²)',
                    value: '0',
                    icon: Icons.area_chart,
                    color: Colors.green,
                  ),
                  const SizedBox(height: 12),
                  _buildStatCard(
                    title: 'Số lớp dữ liệu',
                    value: '0',
                    icon: Icons.layers,
                    color: Colors.orange,
                  ),
                ],
              ),
            ),
          ),
        ],
      ),
    );
  }

  Widget _buildStatCard({
    required String title,
    required String value,
    required IconData icon,
    required Color color,
  }) {
    return Container(
      padding: const EdgeInsets.all(16),
      decoration: BoxDecoration(
        color: Colors.white,
        border: Border.all(color: const Color(0xFFE1DFDD)),
        borderRadius: BorderRadius.circular(4),
        boxShadow: [
          BoxShadow(
            color: Colors.black.withOpacity(0.05),
            blurRadius: 4,
            offset: const Offset(0, 2),
          ),
        ],
      ),
      child: Row(
        children: [
          Container(
            width: 48,
            height: 48,
            decoration: BoxDecoration(
              color: color.withOpacity(0.1),
              borderRadius: BorderRadius.circular(8),
            ),
            child: Icon(icon, color: color, size: 28),
          ),
          const SizedBox(width: 16),
          Expanded(
            child: Column(
              crossAxisAlignment: CrossAxisAlignment.start,
              children: [
                Text(
                  title,
                  style: const TextStyle(
                    fontSize: 14,
                    color: Color(0xFF605E5C),
                  ),
                ),
                const SizedBox(height: 4),
                Text(
                  value,
                  style: const TextStyle(
                    fontSize: 24,
                    fontWeight: FontWeight.bold,
                    color: Color(0xFF323130),
                  ),
                ),
              ],
            ),
          ),
        ],
      ),
    );
  }
}
```

#### **Bước 4**: Import và thêm vào TabBarView

Trong `home_page.dart`:

**Import file mới** (thêm vào đầu file):

```dart
import 'widgets/tabs/statistics/statistics.dart';
```

**Thêm vào TabBarView** (dòng 698-714):

```dart
// TRƯỚC
child: TabBarView(
  physics: const NeverScrollableScrollPhysics(),
  controller: widget._tabController,
  children: [
    LayerTab(controller: widget.controller),
    BlocProvider(
      create: (context) => getIt<SearchBloc>(),
      child: SearchTab(controller: widget.controller),
    ),
    DirectionTab(controller: widget.controller),
    BlocProvider(
      create: (context) => getIt<FilterCubit>(),
      child: Filter(controller: widget.controller),
    ),
    const GuideTab(),
  ],
),

// SAU - Thêm StatisticsTab
child: TabBarView(
  physics: const NeverScrollableScrollPhysics(),
  controller: widget._tabController,
  children: [
    LayerTab(controller: widget.controller),
    BlocProvider(
      create: (context) => getIt<SearchBloc>(),
      child: SearchTab(controller: widget.controller),
    ),
    DirectionTab(controller: widget.controller),
    BlocProvider(
      create: (context) => getIt<FilterCubit>(),
      child: Filter(controller: widget.controller),
    ),
    const GuideTab(),
    StatisticsTab(controller: widget.controller), // THÊM DÒNG NÀY
  ],
),
```

#### **Bước 5**: Cập nhật số lượng tabs

Trong `_buildVerticalTab` (dòng 728-733):

```dart
// TRƯỚC
child: ListView.separated(
  itemBuilder: (_, i) => _buildVerticalTab(i),
  separatorBuilder: (_, __) => const SizedBox(height: 2),
  itemCount: 5,  // Cũ: 5 tabs
),

// SAU
child: ListView.separated(
  itemBuilder: (_, i) => _buildVerticalTab(i),
  separatorBuilder: (_, __) => const SizedBox(height: 2),
  itemCount: 6,  // Mới: 6 tabs
),
```

#### **Bước 6**: Lưu và chạy lại

```bash
flutter run -d chrome
```

---

## 6. TÙY CHỈNH GIAO DIỆN

### 🎨 Màu sắc chính

File: `example/lib/src/common/themes/app_colors.dart`

```dart
// Màu chủ đạo (Word 2010 style)
const primaryBlue = Color(0xFF0078D4);      // Màu xanh chính
const lightGray = Color(0xFFF3F2F1);        // Nền xám nhạt
const mediumGray = Color(0xFF8A8886);       // Xám trung bình
const darkGray = Color(0xFF323130);         // Xám đậm
const borderGray = Color(0xFFE1DFDD);       // Viền xám
```

### 📏 Kích thước

Trong `home_page.dart`:

```dart
// Chiều rộng menu bên phải (dòng 680-685)
width: isMobile
    ? MediaQuery.of(context).size.width * 0.25  // 25% trên mobile
    : MediaQuery.of(context).size.width * 0.19, // 19% trên desktop

// Chiều cao tab menu dọc (dòng 753)
height: isMobile ? 50 : 60,

// Font size tab menu (dòng 774)
fontSize: isMobile ? 10 : 12,
```

### ⚙️ Thay đổi kiểu hiển thị

**Thay đổi từ menu dọc sang menu ngang:**

Hiện tại menu được đặt bên phải dọc. Để chuyển sang ngang, cần:

1. Di chuyển phần tab từ `Row` sang `Column`
2. Thay đổi layout trong `HomeActionWidget`
3. Uncomment phần `WordRibbon` trong code (dòng 193-336)

---

## 7. CẤU TRÚC CHI TIẾT CÁC TAB

### 📂 LayerTab (Lớp bản đồ)

- **File**: `widgets/tabs/layer.dart`
- **Chức năng**: Hiển thị và quản lý các lớp bản đồ
- **Có thể**: Bật/tắt layer, thay đổi độ mờ

### 🔍 SearchTab (Tìm kiếm)

- **File**: `widgets/tabs/search/search.dart`
- **Chức năng**: Tìm kiếm địa điểm
- **State Management**: SearchBloc

### 🗺️ DirectionTab (Tìm đường)

- **File**: `widgets/tabs/direction.dart`
- **Chức năng**: Tìm đường giữa 2 điểm

### 🎯 FilterTab (Lọc)

- **File**: `widgets/tabs/filter/filter.dart`
- **Chức năng**: Lọc dữ liệu theo điều kiện
- **State Management**: FilterCubit

### 📖 GuideTab (Hướng dẫn)

- **File**: `widgets/tabs/guide/guide.dart`
- **Chức năng**: Hướng dẫn sử dụng

---

## 8. LƯU Ý QUAN TRỌNG

### ⚠️ Khi thêm tab mới

1. **Luôn đảm bảo**: Số lượng trong `_actions` = số children trong `TabBarView` = `itemCount` trong ListView
2. **Đặt tên file**: Theo chuẩn `snake_case.dart`
3. **Đặt tên class**: Theo chuẩn `PascalCase`
4. **Import đầy đủ**: Các dependencies cần thiết

### 🐛 Debug thường gặp

```dart
// Lỗi: RangeError (index): Invalid value: Not in inclusive range 0..4: 5
// Nguyên nhân: Thêm tab trong _actions nhưng chưa thêm widget tương ứng

// Giải pháp: Kiểm tra lại:
// 1. Số phần tử trong _actions
// 2. Số children trong TabBarView
// 3. itemCount trong ListView.separated
```

### 📚 Tài liệu tham khảo

- Flutter Documentation: <https://flutter.dev/docs>
- ArcGIS SDK for JavaScript: <https://developers.arcgis.com/javascript/>
- Flutter Bloc: <https://bloclibrary.dev/>

---

## 9. HỖ TRỢ VÀ BẢO TRÌ

### 🔄 Cập nhật sau này

Khi cần thêm tính năng mới cho các xã khác:

1. Clone dự án hiện tại
2. Thay đổi assets (logo, hình ảnh)
3. Cập nhật layers trong ArcGIS Online
4. Thay đổi URL layers trong code
5. Build và deploy

### 📞 Liên hệ hỗ trợ

- Khi gặp lỗi: Kiểm tra console log
- Khi cần thêm tính năng: Tham khảo cấu trúc tabs hiện có
- Khi deploy: Đảm bảo đường dẫn assets đúng

---

**Cập nhật lần cuối**: 21/10/2025
**Phiên bản**: 1.0
**Tác giả**: Ho Doan
