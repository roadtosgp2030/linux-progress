# Giai đoạn 1 — Nền tảng Linux

## 1. Cấu trúc thư mục gốc (/)

| Thư mục         | Ý nghĩa                                      |
| --------------- | -------------------------------------------- |
| `/home`         | Thư mục của user (`/home/ducwe`)             |
| `/etc`          | Config hệ thống và service (nginx, ssh...)   |
| `/var`          | Log, data thay đổi thường xuyên (`/var/log`) |
| `/usr`          | Chương trình đã cài (`/usr/bin`)             |
| `/tmp`          | File tạm, tự xóa khi reboot                  |
| `/proc`         | Thông tin process đang chạy (ảo)             |
| `/bin`, `/sbin` | Lệnh hệ thống cơ bản                         |
| `/root`         | Thư mục home của user root                   |

> `~` là shortcut trỏ đến `/home/ducwe`

---

## 2. Điều hướng file system

```bash
pwd          # xem đang ở thư mục nào
ls           # xem danh sách file/thư mục
ls -la       # xem chi tiết (permissions, size, date)
cd /etc      # di chuyển vào thư mục
cd ~         # về home directory
```

### Đọc output của `ls -la`

```
drwxr-xr-x  2  ducwe  ducwe  4096  May 1  .
│││││││││││     │      │
│││││││││││     │      └─ group
│││││││││││     └──────── owner
││└┴┴┴┴┴┴┴┘
│└────────── d=directory, -=file, l=symlink
└─────────── permissions (rwx)
```

---

## 3. Quản lý file và thư mục

```bash
mkdir linux-practice        # tạo thư mục
touch file1.txt             # tạo file rỗng
cp file1.txt file1-bak.txt  # copy file
cp -r folder folder-bak     # copy thư mục
mv file2.txt file2-new.txt  # đổi tên hoặc di chuyển
rm file3.txt                # xóa file
rm -r folder                # xóa thư mục (không vào thùng rác!)
rmdir folder                # xóa thư mục rỗng
```

> `rm -r` xóa vĩnh viễn, không khôi phục được — cẩn thận trên server thật!

---

## 4. Xem nội dung file

```bash
cat file1.txt           # xem toàn bộ file
head -n 2 file1.txt     # xem 2 dòng đầu
tail -n 3 file1.txt     # xem 3 dòng cuối
tail -f file1.txt       # theo dõi file realtime (Ctrl+C để thoát)
```

### Ghi nội dung vào file

```bash
echo "nội dung" > file.txt    # ghi đè
echo "nội dung" >> file.txt   # thêm vào cuối
```

---

## 5. Tìm kiếm với grep

```bash
grep "linux" file1.txt          # tìm dòng chứa "linux"
grep -i "LINUX" file1.txt       # không phân biệt hoa thường
grep -n "linux" file1.txt       # hiện số dòng
grep -v "linux" file1.txt       # dòng KHÔNG chứa "linux"

# Kết hợp với pipe
tail -f file1.txt | grep "ERROR"   # lọc log realtime
```

### Pipe `|`

Lấy output của lệnh bên trái làm input cho lệnh bên phải:

```bash
cat file1.txt | grep "linux" | head -n 1
```

---

## 6. Phân quyền file

### Cấu trúc permissions

```
-  rw-  r--  r--
│   │    │    │
│   │    │    └─ others
│   │    └─────  group
│   └──────────  owner
└──────────────  loại (- file, d dir, l link)
```

| Ký tự | Quyền    | Giá trị số |
| ----- | -------- | ---------- |
| `r`   | read     | 4          |
| `w`   | write    | 2          |
| `x`   | execute  | 1          |
| `-`   | không có | 0          |

### chmod

```bash
chmod u+x script.sh      # thêm quyền thực thi cho owner
chmod go-w script.sh     # bỏ quyền ghi của group và others
chmod 755 script.sh      # rwxr-xr-x (script, thư mục)
chmod 644 file.txt       # rw-r--r-- (file thông thường)
chmod 600 .env           # rw------- (file bảo mật)
```

### Quyền hay gặp

| Chmod | Permissions | Dùng cho                  |
| ----- | ----------- | ------------------------- |
| `755` | `rwxr-xr-x` | Script, thư mục           |
| `644` | `rw-r--r--` | File thông thường, config |
| `600` | `rw-------` | File bảo mật (.env, key)  |

### chown

```bash
chown ducwe file.txt            # đổi owner
chown ducwe:ducwe file.txt      # đổi owner và group
sudo chown root:root file.txt   # cần sudo khi đổi sang root
```

### Group

```bash
groups          # xem bạn thuộc group nào
```

- Mỗi user có một group mặc định cùng tên
- Group `sudo` = được phép chạy lệnh với `sudo`
- Hay gặp: `sudo usermod -aG docker ducwe` để chạy Docker không cần sudo

---

## Nguyên tắc quan trọng

- **Principle of least privilege:** cấp đúng quyền cần thiết, không hơn không kém
- **`rm -r` không có thùng rác** — cẩn thận trên server thật
- **`sudo`** = chạy với quyền admin, chỉ dùng khi cần thiết

Học lần đầu: 01/05/2026
Revise lần 1: 02/05/2026
