# Rent Connect

Rent Connect là ứng dụng Flutter hỗ trợ tìm kiếm và quản lý tin cho thuê phòng trọ/bất động sản. Ứng dụng cho phép người dùng đăng ký tài khoản, đăng nhập, tìm kiếm tin đăng, xem chi tiết phòng cho thuê và quản lý các tin đã đăng.

## Mục lục

- [Tổng quan dự án](#tổng-quan-dự-án)
- [Tính năng chính](#tính-năng-chính)
- [Công nghệ sử dụng](#công-nghệ-sử-dụng)
- [Cấu trúc thư mục](#cấu-trúc-thư-mục)
- [Yêu cầu môi trường](#yêu-cầu-môi-trường)
- [Cài đặt và chạy dự án](#cài-đặt-và-chạy-dự-án)
- [Cấu hình Supabase](#cấu-hình-supabase)
- [Các lỗi thường gặp](#các-lỗi-thường-gặp)
- [Tài liệu BA](#tài-liệu-ba)

## Tổng quan dự án

Rent Connect hướng tới hai nhóm người dùng chính:

- **Tenant**: người thuê phòng, có nhu cầu tìm kiếm và xem thông tin phòng cho thuê.
- **Landlord**: người cho thuê, có nhu cầu đăng tin, cập nhật tin và quản lý các tin đã đăng.

Ứng dụng sử dụng Supabase để xử lý xác thực người dùng, lưu dữ liệu tin đăng, lưu thông tin hồ sơ và lưu ảnh tin đăng.

## Tính năng chính

- **Đăng ký tài khoản**: tạo tài khoản bằng email, mật khẩu, họ tên, số điện thoại, địa chỉ và vai trò người dùng.
- **Đăng nhập/đăng xuất**: xác thực người dùng bằng Supabase Authentication.
- **Trang chủ**: hiển thị các nhóm tin đăng như dành cho bạn, gần bạn và mới nhất.
- **Tìm kiếm tin đăng**: tìm theo tiêu đề, địa chỉ, xã/phường, quận/huyện hoặc tỉnh/thành phố.
- **Xem chi tiết tin đăng**: hiển thị ảnh, tiêu đề, địa chỉ, diện tích, giá thuê, tiền cọc, mô tả và số điện thoại liên hệ.
- **Đăng tin cho thuê**: người dùng đã đăng nhập có thể tạo tin mới kèm ảnh và thông tin chi tiết.
- **Quản lý tin đã đăng**: xem danh sách tin của người dùng hiện tại.
- **Cập nhật tin đăng**: chỉnh sửa thông tin phòng và danh sách ảnh.
- **Xóa tin đăng**: xóa tin khỏi cơ sở dữ liệu.
- **Quản lý hồ sơ**: xem thông tin cá nhân của người dùng.
- **Chat**: hiện đang là placeholder, chưa hoàn thiện luồng chat realtime.

## Công nghệ sử dụng

- **Flutter/Dart**: xây dựng ứng dụng mobile.
- **Riverpod**: quản lý trạng thái.
- **GoRouter**: điều hướng màn hình.
- **Supabase Authentication**: đăng ký, đăng nhập và đăng xuất.
- **Supabase Database**: lưu dữ liệu người dùng và tin đăng.
- **Supabase Storage**: lưu ảnh tin đăng.
- **SharedPreferences**: lưu thông tin phiên/người dùng cục bộ.
- **Image Picker**: chọn ảnh từ thiết bị.
- **Intl**: định dạng tiền tệ.

## Cấu trúc thư mục

```text
lib/
├── app_router.dart
├── constants.dart
├── main.dart
├── core/
│   ├── providers/
│   └── widgets/
├── features/
│   └── auth/
│       ├── controller/
│       ├── model/
│       ├── repositories/
│       ├── services/
│       └── views/
└── utils/
```

Một số thư mục quan trọng:

- `lib/features/auth/views`: các màn hình chính như đăng nhập, đăng ký, trang chủ, tìm kiếm, đăng tin, quản lý tin.
- `lib/features/auth/model`: model dữ liệu `UserModel`, `PostModel`, `PostDetailsModel`.
- `lib/features/auth/repositories`: lớp truy cập Supabase.
- `lib/features/auth/services`: xử lý logic nghiệp vụ trung gian.
- `lib/core/providers`: khai báo Riverpod provider.
- `lib/core/widgets`: widget dùng chung.
- `docs/ba`: tài liệu Business Analysis của dự án.

## Yêu cầu môi trường

- Flutter SDK tương thích Dart SDK `^3.8.0`.
- Android Studio hoặc VS Code có Flutter extension.
- Thiết bị thật hoặc emulator Android/iOS.
- Tài khoản Supabase.
- Git.

Kiểm tra môi trường Flutter:

```bash
flutter doctor
```

## Cài đặt và chạy dự án

### 1. Clone repository

```bash
git clone https://github.com/NhatMinh171003/rent_connect.git
cd rent_connect
```

### 2. Cài dependencies

```bash
flutter pub get
```

### 3. Tạo file `.env`

Tạo file `.env` ở thư mục gốc dự án:

```env
SUPABASE_URL=your_supabase_project_url
SUPABASE_ANON_KEY=your_supabase_anon_key
```

Bạn có thể xem file mẫu tại `.env.example`.

### 4. Chạy ứng dụng

```bash
flutter run
```

Nếu có nhiều thiết bị:

```bash
flutter devices
flutter run -d <device_id>
```

## Cấu hình Supabase

Ứng dụng cần các thành phần Supabase sau.

### Authentication

Bật Email Authentication trong Supabase.

### Database tables

#### `tbl_user`

| Trường | Ghi chú |
|---|---|
| `user_id` | ID người dùng từ Supabase Auth |
| `user_name` | Họ tên |
| `phone_number` | Số điện thoại |
| `commune` | Xã/phường |
| `district` | Quận/huyện |
| `city` | Tỉnh/thành phố |
| `role` | `tenant` hoặc `landlord` |
| `avatar` | URL ảnh đại diện, có thể để trống |

#### `tbl_post`

| Trường | Ghi chú |
|---|---|
| `id` | ID tin đăng |
| `user_id` | ID người đăng |
| `title` | Tiêu đề |
| `description` | Mô tả |
| `area` | Diện tích |
| `deposit` | Tiền cọc |
| `price` | Giá thuê |
| `address` | Địa chỉ chi tiết |
| `commune` | Xã/phường |
| `district` | Quận/huyện |
| `city` | Tỉnh/thành phố |
| `image` | Danh sách URL ảnh |

### Storage buckets

Ứng dụng đang upload ảnh tin đăng vào bucket:

```text
post_img
```

Lưu ý: trong code hiện tại có một số đoạn xóa ảnh tham chiếu bucket khác như `post_images` hoặc `tbl_post`. Nếu chức năng xóa ảnh không hoạt động đúng, cần đồng bộ lại tên bucket trong `PostRepository`.

## Các lỗi thường gặp

### 1. Lỗi không tìm thấy file `.env`

Thông báo có thể gặp:

```text
Instance of 'FileNotFoundError'
```

Cách xử lý:

- Kiểm tra file `.env` đã nằm ở thư mục gốc dự án chưa.
- Chạy lại:

```bash
flutter pub get
flutter run
```

### 2. Lỗi `Null check operator used on a null value` khi khởi tạo Supabase

Nguyên nhân thường là thiếu biến:

```env
SUPABASE_URL
SUPABASE_ANON_KEY
```

Cách xử lý:

- Kiểm tra đúng tên biến trong file `.env`.
- Không thêm dấu nháy không cần thiết quanh giá trị.

### 3. Không đăng ký hoặc đăng nhập được

Nguyên nhân có thể:

- Supabase URL hoặc anon key sai.
- Email Authentication chưa được bật.
- Bảng `tbl_user` chưa tạo hoặc thiếu cột.
- Row Level Security policy chưa cho phép insert/select phù hợp.

Cách xử lý:

- Kiểm tra Authentication trong Supabase.
- Kiểm tra log trong Supabase.
- Kiểm tra bảng `tbl_user`.

### 4. Không tạo được tin đăng

Nguyên nhân có thể:

- Bảng `tbl_post` chưa tồn tại hoặc thiếu cột.
- Kiểu dữ liệu `area`, `deposit`, `price` không đúng.
- User chưa đăng nhập.
- Policy của Supabase không cho insert.

Cách xử lý:

- Kiểm tra bảng `tbl_post`.
- Kiểm tra RLS policy.
- Kiểm tra console log khi submit form đăng tin.

### 5. Upload ảnh thất bại

Nguyên nhân có thể:

- Bucket `post_img` chưa tồn tại.
- Bucket chưa public hoặc policy chưa cho phép upload.
- Ứng dụng chưa có quyền truy cập ảnh trên thiết bị.

Cách xử lý:

- Tạo bucket `post_img` trong Supabase Storage.
- Kiểm tra Storage policy.
- Kiểm tra quyền đọc ảnh trên thiết bị/emulator.

### 6. Ảnh hiển thị lỗi hoặc không load được

Nguyên nhân có thể:

- URL ảnh không public.
- Bucket chưa public.
- File ảnh đã bị xóa khỏi Supabase Storage.

Cách xử lý:

- Kiểm tra URL ảnh trong dữ liệu `tbl_post`.
- Kiểm tra quyền public của bucket.

### 7. Tìm kiếm không trả kết quả

Nguyên nhân có thể:

- Từ khóa rỗng.
- Không có dữ liệu khớp trong `title`, `address`, `city`, `commune`, `district`.
- Supabase query bị chặn bởi policy.

Cách xử lý:

- Thử tìm bằng tên thành phố/quận/huyện đã có trong database.
- Kiểm tra dữ liệu trong `tbl_post`.
- Kiểm tra policy select của bảng `tbl_post`.

### 8. Lỗi dependency Flutter

Cách xử lý thường dùng:

```bash
flutter clean
flutter pub get
flutter run
```

## Tài liệu BA

Dự án có bộ tài liệu Business Analysis trong thư mục [`docs/ba`](docs/ba), bao gồm:

- SRS overview.
- Use case specification.
- User story và acceptance criteria.
- Data model và validation rules.
- Activity diagrams bằng file `.drawio`.

## Ghi chú phát triển

- File `.env` không được commit lên GitHub.
- Khi thêm biến môi trường mới, hãy cập nhật `.env.example`.
- Khi thay đổi database schema, hãy cập nhật lại tài liệu trong `docs/ba`.
- Một số chức năng như chat và notification hiện chưa hoàn thiện.
