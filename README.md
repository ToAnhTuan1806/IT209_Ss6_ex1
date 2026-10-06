# Báo Cáo Bài 1: Phân Quyền Thư Mục Ứng Dụng Web

## 1. Nhật ký các lệnh đã thực hiện

### Bước 1: Tạo cấu trúc thư mục
Tạo thư mục `/var/www/my-app/public` (chứa trang tĩnh) và `/var/www/my-app/logs` (chứa log):
```bash
sudo mkdir -p /var/www/my-app/public
sudo mkdir -p /var/www/my-app/logs
```

### Bước 2: Gán quyền sở hữu (Owner & Group)
Chuyển chủ sở hữu (Owner) về tài khoản thường (`$USER`) và nhóm sở hữu (Group) về nhóm Web `www-data`:
```bash
sudo chown -R $USER:www-data /var/www/my-app
```

### Bước 3: Phân quyền truy cập (chmod)
- **Thư mục `public` (quyền 750 / drwxr-x---):**
  - Owner có quyền đọc, ghi, điều hướng (`rwx` = 7).
  - Group chỉ có quyền đọc và điều hướng (`r-x` = 5).
  - Others không có quyền (`---` = 0).
  ```bash
  sudo chmod 750 /var/www/my-app/public
  ```

- **Thư mục `logs` (quyền 770 / drwxrwx---):**
  - Cả Owner và Group đều có toàn quyền đọc, ghi, điều hướng (`rwx` = 7).
  - Others không có quyền (`---` = 0).
  ```bash
  sudo chmod 770 /var/www/my-app/logs
  ```

---

## 2. Kết quả kiểm tra (`ls -la /var/www/my-app`)

Chạy lệnh kiểm tra cấu trúc và phân quyền:
```bash
ls -la /var/www/my-app
```

**Đầu ra kết quả:**
```console
$ ls -la /var/www/my-app
total 16
drwxr-xr-x 4 ubuntu www-data 4096 Oct  6 20:30 .
drwxr-xr-x 3 root   root     4096 Oct  6 20:25 ..
drwxrwx--- 2 ubuntu www-data 4096 Oct  6 20:30 logs
drwxr-x--- 2 ubuntu www-data 4096 Oct  6 20:30 public
```

---

## 3. Giải thích kết quả

- **`/var/www/my-app/public` (`drwxr-x---` - 750):** 
  - Chủ sở hữu (`ubuntu`): Toàn quyền (`rwx`).
  - Nhóm (`www-data`): Quyền đọc và truy cập (`r-x`) để máy chủ web đọc được file tĩnh.
  - Người dùng khác: Không có quyền truy cập (`---`).

- **`/var/www/my-app/logs` (`drwxrwx---` - 770):** 
  - Cả chủ sở hữu (`ubuntu`) và nhóm (`www-data`): Có toàn quyền (`rwx`) để ghi và đọc file nhật ký.
  - Người dùng khác: Bị chặn hoàn toàn (`---`).
