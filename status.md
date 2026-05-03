# Linux Learning Progress

## Lộ trình: Linux cho Backend Developer

---

## Giai đoạn 1 — Nền tảng
> Tài liệu: `stage1.md` | Học: 01/05/2026 | Ôn: 02/05/2026

- [x] Cấu trúc thư mục gốc (`/etc`, `/var`, `/home`, `/tmp`...)
- [x] Điều hướng file system (`pwd`, `ls`, `cd`)
- [x] Quản lý file và thư mục (`cp`, `mv`, `rm`, `mkdir`, `touch`)
- [x] Xem nội dung file (`cat`, `head`, `tail`, `tail -f`)
- [x] Tìm kiếm với `grep` và pipe `|`
- [x] Phân quyền file (`chmod`, `chown`, `groups`)

---

## Giai đoạn 2 — Vận hành hệ thống
> Tài liệu: `stage2.md` | Học: 02–03/05/2026 | Ôn: 04/05/2026

- [x] Process: `ps`, `top`, `kill`, `pkill`, `jobs`
- [x] Network: `ping`, `ss`, `curl`
- [x] Disk & Memory: `df`, `du`, `free`
- [x] Log: `tail -f /var/log/syslog`, `journalctl`
- [x] Service: `systemctl start/stop/status/enable`

---

## Giai đoạn 3 — Kết hợp Docker
> Song song với Giai đoạn 2

- [ ] Linux namespaces & cgroups (nền tảng của container)
- [ ] `docker exec` vào container và dùng lệnh Linux bên trong
- [ ] Đọc hiểu `Dockerfile`: `RUN`, `COPY`, `WORKDIR`
- [ ] Xem log container: `docker logs -f`
- [ ] Mount volume và Linux file permissions

---

## Giai đoạn 4 — Shell scripting cơ bản

- [ ] Biến, vòng lặp, điều kiện
- [ ] Pipe `|` và redirect `>`, `>>`
- [ ] Viết script thực tế (backup, deploy, health check)

---

## Tiến độ tổng quan

| Giai đoạn | Trạng thái |
|-----------|-----------|
| Giai đoạn 1 — Nền tảng | Hoàn thành |
| Giai đoạn 2 — Vận hành hệ thống | Hoàn thành |
| Giai đoạn 3 — Docker | Chưa bắt đầu |
| Giai đoạn 4 — Shell scripting | Chưa bắt đầu |

---

## Phương pháp học

**Ôn tập ngắt quãng (Spaced Repetition):** sau khi học xong một phần, ôn lại theo chu kỳ: **+1 ngày → +3 ngày → +7 ngày**.

### Lịch ôn tập hiện tại

| Nội dung | Học lúc | Ôn lần 1 (+1 ngày) | Ôn lần 2 (+3 ngày) | Ôn lần 3 (+7 ngày) |
|---|---|---|---|---|
| Stage 1 | 01/05/2026 | 02/05 ✅ | 05/05 | 08/05 |
| Stage 2 (3/5 đầu) | 02/05/2026 | 03/05 ✅ | 05/05 | 09/05 |
| Stage 2 (2/5 còn lại) | 03/05/2026 | 04/05 | 06/05 | 10/05 |

> Khi bắt đầu session mới, hãy tính xem hôm nay có nội dung nào đến hạn ôn không, sau đó mới học tiếp phần mới.

---

## Cách dùng file này

Paste nội dung file này vào đầu hội thoại mới để Claude biết bạn đang học đến đâu.
Ví dụ: *"Đây là tiến độ học Linux của tôi, hãy tiếp tục từ chỗ còn dở."*
