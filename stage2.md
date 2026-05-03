# Giai đoạn 2 — Vận hành hệ thống

## 1. Process

### Xem process

```bash
ps                  # process trong terminal hiện  tại
ps aux              # tất cả process của mọi user
top                 # realtime (giống Task Manager)
```

### Các cột quan trọng trong `ps aux`

| Cột       | Ý nghĩa                            |
| --------- | ---------------------------------- |
| `PID`     | Process ID — số định danh duy nhất |
| `USER`    | User đang chạy process             |
| `%CPU`    | Phần trăm CPU đang dùng            |
| `%MEM`    | Phần trăm RAM đang dùng            |
| `STAT`    | Trạng thái (S=sleeping, R=running) |
| `COMMAND` | Lệnh đang chạy                     |

### Phím tắt trong `top`

| Phím | Ý nghĩa       |
| ---- | ------------- |
| `q`  | Thoát         |
| `M`  | Sort theo RAM |
| `P`  | Sort theo CPU |

### Chạy process background

```bash
sleep 1000 &        # & = chạy ngầm
jobs                # xem các process background
```

Output của `jobs`:

- `[1]`, `[2]` — số thứ tự job
- `+` — job hiện tại (mới nhất)
- `-` — job trước đó

### Kill process

```bash
kill <PID>          # gửi SIGTERM — yêu cầu tắt lịch sự
kill -9 <PID>       # gửi SIGKILL — tắt ngay lập tức
pkill sleep         # kill theo tên process (không cần PID)
```

> **Quy tắc:** dùng `kill` trước, nếu không chết mới dùng `kill -9`

**Sự khác biệt SIGTERM vs SIGKILL:**
| | `kill` (SIGTERM) | `kill -9` (SIGKILL) |
|---|---|---|
| Web server đang xử lý request | Chờ request xong rồi tắt | Tắt ngay, client bị lỗi |
| Database đang ghi | Flush data rồi tắt | Có thể corrupt data |
| RAM | Giải phóng | Giải phóng (kernel tự thu hồi) |

---

## 2. Network

### ping — kiểm tra kết nối

```bash
ping google.com             # ping liên tục (Ctrl+C để dừng)
ping -c 4 google.com        # ping 4 lần rồi dừng
```

### ss — xem port đang lắng nghe

```bash
ss -tlnp
# -t: TCP, -l: listening, -n: số thay tên, -p: process
```

Khi chạy backend sẽ thấy port app xuất hiện ở đây.

### curl — gọi HTTP request (giống Postman trong terminal)

```bash
# GET request
curl https://httpbin.org/get

# POST request với JSON body
curl -X POST https://httpbin.org/post \
  -H "Content-Type: application/json" \
  -d '{"name": "ducwe", "role": "backend dev"}'

# Chỉ xem response headers
curl -I https://httpbin.org/get
```

**Các flag hay dùng:**
| Flag | Ý nghĩa |
|------|---------|
| `-X POST` | Chỉ định HTTP method |
| `-H` | Thêm header |
| `-d` | Request body |
| `-I` | Chỉ xem response headers |
| `-s` | Silent — ẩn progress bar |
| `-o file.json` | Lưu response ra file |

---

## 3. Disk & Memory

### df — dung lượng ổ đĩa

```bash
df -h               # -h = human readable (GB, MB thay vì bytes)
```

> Trong WSL: `/mnt/c`, `/mnt/d` là ổ Windows được mount vào Linux

### du — thư mục nào chiếm nhiều dung lượng

```bash
du -sh ~/*          # -s = summary, -h = human readable
du -sh /var/log/*   # xem log đang chiếm bao nhiêu
```

### free — RAM

```bash
free -h
```

| Cột          | Ý nghĩa                                      |
| ------------ | -------------------------------------------- |
| `total`      | Tổng RAM                                     |
| `used`       | Đang dùng                                    |
| `free`       | Còn trống                                    |
| `buff/cache` | Cache của kernel (có thể giải phóng khi cần) |
| `available`  | Thực sự có thể dùng được                     |

---

## 4. Log

### journalctl — đọc system journal

systemd ghi toàn bộ log vào một binary database gọi là *journal*. `journalctl` là công cụ để query database đó.

```bash
journalctl -n 20 --no-pager        # 20 dòng gần nhất, không dùng pager
journalctl -f                       # follow real-time (Ctrl+C để thoát)
journalctl -u nginx --no-pager     # chỉ log của một service (-u = unit)
journalctl --since "5 minutes ago" --no-pager
journalctl --since "14:00" --until "14:30" --no-pager
```

**Cấu trúc mỗi dòng log:**
```
May 03 13:46:08   vuduc   systemd[1]:   Starting motd-news.service...
   └─ thời gian   └─ host  └─ service[PID]   └─ nội dung
```

| Lệnh | Dùng khi |
|---|---|
| `journalctl -n 50 --no-pager` | Xem nhanh log gần nhất |
| `journalctl -f` | Theo dõi real-time khi deploy |
| `journalctl -u nginx` | Chỉ xem log của một service |
| `journalctl --since "5 minutes ago"` | Khoanh vùng theo thời gian |

---

## 5. Service

### systemctl — quản lý service

```bash
systemctl status nginx              # xem trạng thái
sudo systemctl start nginx          # khởi động
sudo systemctl stop nginx           # dừng
sudo systemctl restart nginx        # restart sau khi thay đổi config
sudo systemctl enable nginx         # tự chạy khi boot
sudo systemctl disable nginx        # không tự chạy khi boot
```

**Đọc output của `systemctl status`:**

```
Loaded: loaded (...; enabled; ...)   ← enabled = tự chạy khi boot
Active: active (running)             ← đang chạy
Active: inactive (dead)              ← đang tắt
Active: failed                       ← bị crash
```

**Phân biệt start vs enable:**

| | Ý nghĩa |
|---|---|
| `start/stop` | Bật/tắt ngay lúc này |
| `enable/disable` | Có tự chạy khi boot không |

> Sau khi deploy, luôn nhớ `sudo systemctl enable ten-service` để app tự sống lại khi server restart.

**nginx có kiến trúc master + worker:**
- 1 master process quản lý
- Nhiều worker process xử lý request (số lượng = số CPU core)

---

Học lần đầu: 02/05/2026
Hoàn thành: 03/05/2026
