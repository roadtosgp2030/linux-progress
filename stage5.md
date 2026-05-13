# Giai đoạn 5 — Linux Networking & Security

## 1. Firewall với `ufw`

`ufw` (Uncomplicated Firewall) là lớp quản lý dễ dùng hơn `iptables`.

```bash
ufw status              # xem trạng thái firewall
ufw enable              # bật firewall
ufw disable             # tắt firewall

ufw allow 22            # cho phép SSH (port 22)
ufw allow 80            # cho phép HTTP
ufw allow 443           # cho phép HTTPS
ufw deny 3306           # chặn MySQL từ ngoài

ufw allow from 192.168.1.0/24   # chỉ cho phép subnet nội bộ
ufw delete allow 80             # xóa rule đã tạo
```

> Luôn `allow 22` trước khi `enable` — nếu không sẽ bị khóa SSH.

### Xem rules chi tiết

```bash
ufw status numbered     # xem rules kèm số thứ tự
ufw delete 2            # xóa rule số 2
```

---

## 2. SSH Hardening

SSH là cổng vào server — cấu hình sai là rủi ro bảo mật lớn nhất.

### Tạo SSH key (trên máy local)

```bash
ssh-keygen -t ed25519 -C "ducwebdev@gmail.com"
# Sinh ra: ~/.ssh/id_ed25519 (private) và ~/.ssh/id_ed25519.pub (public)

# Copy public key lên server
ssh-copy-id user@server-ip
# hoặc thủ công:
cat ~/.ssh/id_ed25519.pub >> ~/.ssh/authorized_keys
```

### File cấu hình SSH server: `/etc/ssh/sshd_config`

```
# Tắt đăng nhập bằng password (chỉ dùng key)
PasswordAuthentication no

# Tắt đăng nhập trực tiếp root
PermitRootLogin no

# Đổi port mặc định (tránh bot scan port 22)
Port 2222

# Chỉ cho phép user cụ thể
AllowUsers ducwe
```

```bash
# Sau khi sửa config, reload SSH service
systemctl reload sshd
```

> Mở terminal thứ 2 test trước khi đóng terminal hiện tại — tránh bị khóa ngoài.

### `fail2ban` — tự động chặn brute force

```bash
apt install fail2ban

# Config tại /etc/fail2ban/jail.local
[sshd]
enabled = true
maxretry = 5        # sai 5 lần
bantime = 3600      # ban 1 tiếng

systemctl enable fail2ban
systemctl start fail2ban

fail2ban-client status sshd     # xem IP đang bị ban
fail2ban-client unban <IP>      # bỏ ban
```

---

## 3. Debug network với `ss` và `tcpdump`

### `ss` — xem socket/port đang lắng nghe

```bash
ss -tlnp            # TCP, Listening, Numeric, Process
# -t = TCP | -u = UDP | -l = listening | -n = không resolve DNS | -p = process name

ss -tlnp | grep 80          # xem ai đang chiếm port 80
ss -s                       # thống kê tổng quan
```

### `tcpdump` — bắt traffic thực tế

```bash
tcpdump -i eth0                         # bắt tất cả traffic trên interface eth0
tcpdump -i eth0 port 80                 # chỉ bắt HTTP
tcpdump -i eth0 host 192.168.1.10       # chỉ bắt traffic từ IP này
tcpdump -i eth0 -w capture.pcap         # lưu ra file (xem bằng Wireshark)
tcpdump -i any port 443 -nn             # -nn = không resolve hostname/port name
```

> `tcpdump` cần quyền root. Dùng `Ctrl+C` để dừng.

---

## 4. DNS và name resolution

```bash
cat /etc/hosts          # ánh xạ hostname → IP tĩnh (ưu tiên hơn DNS)
cat /etc/resolv.conf    # DNS server đang dùng

# Tra cứu DNS
nslookup google.com
dig google.com          # chi tiết hơn nslookup
dig @8.8.8.8 google.com # tra bằng DNS server cụ thể

# Xem hostname của máy
hostname
hostname -I             # xem tất cả IP của máy
```

### `/etc/hosts` — ghi đè DNS

```
127.0.0.1   localhost
192.168.1.50  myapp.local    # trỏ domain tự tạo về IP nội bộ
```

---

## 5. Kiểm tra kết nối và routing

```bash
ping -c 4 google.com            # ping 4 lần rồi dừng
traceroute google.com           # xem đường đi của gói tin
mtr google.com                  # ping + traceroute realtime

ip addr                         # xem IP các interface
ip route                        # xem routing table
ip route show default           # xem default gateway

curl -v https://example.com     # debug HTTP request (xem header, SSL...)
curl -I https://example.com     # chỉ xem response header
curl -o /dev/null -s -w "%{http_code}" https://example.com  # chỉ lấy status code
```

---

## Tổng kết

| Nhóm | Lệnh chính |
|------|-----------|
| Firewall | `ufw allow/deny/status` |
| SSH security | `sshd_config`, `fail2ban` |
| Debug port | `ss -tlnp` |
| Bắt traffic | `tcpdump -i eth0 port X` |
| DNS | `dig`, `nslookup`, `/etc/hosts` |
| Kết nối | `ping`, `traceroute`, `ip route` |
