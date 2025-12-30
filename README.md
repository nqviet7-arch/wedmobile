# TechStore - Hệ thống Bán Điện Thoại và Laptop

Hệ thống website thương mại điện tử bán điện thoại di động và laptop được xây dựng bằng Laravel với đầy đủ chức năng cho cả admin và người dùng.

## Tính năng chính

### Cho người dùng:
- ✅ Đăng ký/Đăng nhập
- ✅ Xem danh sách sản phẩm (điện thoại, laptop)
- ✅ Tìm kiếm và lọc sản phẩm
- ✅ Xem chi tiết sản phẩm
- ✅ Thêm sản phẩm vào giỏ hàng
- ✅ Quản lý giỏ hàng (cập nhật số lượng, xóa sản phẩm)
- ✅ Đặt hàng và thanh toán
- ✅ Xem lịch sử đơn hàng
- ✅ Xem chi tiết đơn hàng

### Cho Admin:
- ✅ Dashboard với thống kê tổng quan
- ✅ Quản lý danh mục sản phẩm (CRUD)
- ✅ Quản lý sản phẩm (CRUD)
- ✅ Quản lý đơn hàng (xem danh sách, chi tiết, cập nhật trạng thái)
- ✅ Quản lý người dùng (xem danh sách, chi tiết, cập nhật vai trò)

## Yêu cầu hệ thống

- PHP >= 8.2
- Composer
- MySQL/PostgreSQL/SQLite
- Node.js & NPM (cho frontend assets)

## Cài đặt

### Bước 1: Clone repository
```bash
git clone <repository-url>
cd wedmobile
```

### Bước 2: Cài đặt dependencies
```bash
composer install
npm install
```

### Bước 3: Cấu hình môi trường
```bash
cp .env.example .env
php artisan key:generate
```

### Bước 4: Cấu hình database

**Nếu sử dụng MySQL (XAMPP):**
Cập nhật file `.env`:
```env
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=wedmobile
DB_USERNAME=root
DB_PASSWORD=
```

**Nếu sử dụng SQLite:**
Cập nhật file `.env`:
```env
DB_CONNECTION=sqlite
# DB_HOST=127.0.0.1
# DB_PORT=3306
# DB_DATABASE=wedmobile
# DB_USERNAME=root
# DB_PASSWORD=
```

Sau đó tạo file database:
```bash
touch database/database.sqlite
```

### Bước 5: Chạy migrations và seeders
```bash
php artisan migrate
php artisan db:seed
```

Lệnh này sẽ:
- Tạo các bảng trong database
- Tạo 3 tài khoản admin mặc định
- Tạo các danh mục và sản phẩm mẫu

### Bước 6: Tạo symbolic link cho storage
```bash
php artisan storage:link
```

Lệnh này tạo liên kết từ `public/storage` đến `storage/app/public` để hiển thị ảnh.

### Bước 7: Build assets (Production)
```bash
npm run build
```

Hoặc chạy development mode với hot reload:
```bash
npm run dev
```

## Cách chạy hệ thống

### Cách 1: Sử dụng Laravel Development Server (Khuyến nghị cho development)

**Terminal 1 - Chạy Laravel server:**
```bash
php artisan serve
```
Server sẽ chạy tại: `http://localhost:8000`

**Terminal 2 - Chạy Vite dev server (nếu dùng npm run dev):**
```bash
npm run dev
```

### Cách 2: Sử dụng XAMPP (Production/Development)

1. **Cấu hình Virtual Host (tùy chọn):**
   - Thêm vào `C:\xampp\apache\conf\extra\httpd-vhosts.conf`:
   ```apache
   <VirtualHost *:80>
       ServerName wedmobile.test
       DocumentRoot "C:/xampp/htdocs/wedmobile/public"
       <Directory "C:/xampp/htdocs/wedmobile/public">
           AllowOverride All
           Require all granted
       </Directory>
   </VirtualHost>
   ```
   - Thêm vào `C:\Windows\System32\drivers\etc\hosts`:
   ```
   127.0.0.1 wedmobile.test
   ```

2. **Khởi động Apache và MySQL trong XAMPP**

3. **Truy cập:**
   - Nếu dùng Virtual Host: `http://wedmobile.test`
   - Nếu không: `http://localhost/wedmobile/public`

### Cách 3: Sử dụng Composer dev script (Tất cả trong một)

```bash
composer run dev
```

Lệnh này sẽ chạy đồng thời:
- Laravel server
- Queue worker
- Log viewer (Pail)
- Vite dev server

## Lưu ý quan trọng

1. **Sau khi clone hoặc pull code mới:**
   ```bash
   composer install
   npm install
   php artisan migrate
   php artisan db:seed
   php artisan storage:link
   npm run build
   ```

2. **Nếu gặp lỗi cache:**
   ```bash
   php artisan optimize:clear
   php artisan config:clear
   php artisan cache:clear
   php artisan view:clear
   php artisan route:clear
   ```

3. **Nếu thay đổi .env:**
   ```bash
   php artisan config:clear
   ```

4. **Nếu thay đổi routes:**
   ```bash
   php artisan route:clear
   ```

## Tài khoản mặc định

Sau khi chạy `php artisan db:seed`, hệ thống sẽ tạo 3 tài khoản admin mặc định:

**Admin 1:**
- Email: `admin@admin.com`
- Password: `password`
- Role: `admin`

**Admin 2:**
- Email: `admin2@admin.com`
- Password: `admin123`
- Role: `admin`

**Admin 3:**
- Email: `admin3@admin.com`
- Password: `admin456`
- Role: `admin`

**Lưu ý:** Để bảo mật, vui lòng đổi mật khẩu admin sau lần đăng nhập đầu tiên!

## Cấu trúc Database

- **users**: Thông tin người dùng (user/admin)
- **categories**: Danh mục sản phẩm
- **products**: Sản phẩm
- **carts**: Giỏ hàng
- **orders**: Đơn hàng
- **order_items**: Chi tiết đơn hàng

## Cấu trúc thư mục

```
app/
├── Http/
│   ├── Controllers/
│   │   ├── Admin/          # Admin controllers
│   │   ├── Auth/           # Authentication controllers
│   │   ├── CartController.php
│   │   ├── HomeController.php
│   │   ├── OrderController.php
│   │   └── ProductController.php
│   └── Middleware/
│       └── AdminMiddleware.php
├── Models/
│   ├── Cart.php
│   ├── Category.php
│   ├── Order.php
│   ├── OrderItem.php
│   ├── Product.php
│   └── User.php
database/
├── migrations/             # Database migrations
└── seeders/               # Database seeders
resources/
└── views/
    ├── admin/             # Admin views
    ├── auth/              # Authentication views
    ├── cart/              # Cart views
    ├── layouts/           # Layout templates
    ├── orders/            # Order views
    ├── products/          # Product views
    └── home.blade.php
routes/
└── web.php                # Web routes
```

## Routes chính

### User Routes:
- `/` - Trang chủ
- `/products` - Danh sách sản phẩm
- `/products/{slug}` - Chi tiết sản phẩm
- `/cart` - Giỏ hàng
- `/orders` - Đơn hàng
- `/orders/checkout` - Thanh toán

### Admin Routes (yêu cầu đăng nhập với role admin):
- `/admin/dashboard` - Dashboard
- `/admin/categories` - Quản lý danh mục
- `/admin/products` - Quản lý sản phẩm
- `/admin/orders` - Quản lý đơn hàng
- `/admin/users` - Quản lý người dùng

## Công nghệ sử dụng

- **Backend**: Laravel 12
- **Frontend**: Blade Templates, Tailwind CSS, Alpine.js
- **Database**: MySQL/SQLite
- **Authentication**: Laravel Breeze
- **Build Tool**: Vite

## Phát triển thêm

Để phát triển thêm tính năng:

1. Thêm migrations cho các bảng mới
2. Tạo Models và Relationships
3. Tạo Controllers và Views
4. Thêm Routes
5. Cập nhật Middleware nếu cần

## Troubleshooting

### Lỗi "Class not found" hoặc "Route not found"
```bash
composer dump-autoload
php artisan optimize:clear
```

### Lỗi "Storage link not found"
```bash
php artisan storage:link
```

### Lỗi "Database connection failed"
- Kiểm tra file `.env` có đúng thông tin database không
- Kiểm tra MySQL/XAMPP đã khởi động chưa
- Kiểm tra database đã được tạo chưa

### Lỗi "Vite manifest not found"
```bash
npm run build
```
hoặc nếu đang development:
```bash
npm run dev
```

### Lỗi "Permission denied" trên Linux/Mac
```bash
chmod -R 775 storage bootstrap/cache
chown -R www-data:www-data storage bootstrap/cache
```

## License

MIT License

## Tác giả

TechStore - Hệ thống E-commerce được xây dựng với Laravel Framework
