# Cách học hiệu quả trong session này

## Dùng prompt này để bắt đầu session mới

> "Tôi đang học Linux theo lộ trình trong `status.md`. Hãy dạy tôi theo phong cách sau:
> - Giải thích ngắn gọn lý thuyết, sau đó đưa lệnh thực hành ngay
> - Chờ tôi paste kết quả terminal rồi mới giải thích output và đi tiếp
> - Khi tôi hỏi ngoài lề, trả lời rồi quay lại bài học
> - Cuối session, lưu nội dung đã học vào file .md tương ứng và cập nhật status.md"

---

## Cách ôn tập theo lần

| Lần ôn | Format |
|--------|--------|
| Lần 1 | Lý thuyết nhanh + thực hành có hướng dẫn |
| Lần 2+ | **Claude ra bài tập → User triển khai → Claude check kết quả** (không nhắc lý thuyết trước) |

---

## Các nguyên tắc trong session này

**1. Lý thuyết tối thiểu, thực hành ngay**
Không giải thích dài dòng trước. Đưa lệnh thực hành, để người học tự thấy kết quả, rồi mới giải thích output thực tế đó.

**2. Chờ kết quả terminal**
Sau mỗi bài tập, chờ người học paste output. Giải thích dựa trên kết quả thực tế của họ, không phải kết quả giả định.

**3. Trả lời câu hỏi ngoài lề, không bỏ qua**
Khi có câu hỏi phát sinh ("cái này nghĩa là gì?", "tại sao lại vậy?"), trả lời đầy đủ rồi mới quay lại bài học. Không nói "sẽ học sau".

**4. Tăng dần độ phức tạp**
Mỗi lệnh học xong → giới thiệu biến thể nâng cao hơn (ví dụ: `rm` → `rm -r` → `rm -ri`).

**5. Kết nối với thực tế**
Luôn giải thích lệnh đang học liên quan thế nào đến Docker/backend/server thật.

**6. Lưu tài liệu sau mỗi buổi**
Cuối session lưu vào `stage{N}.md` và cập nhật checklist trong `status.md`.

---

## Cấu trúc file tài liệu

```
linux-playground/
├── status.md          ← tiến độ tổng quan, dùng để bắt đầu session mới
├── stage1.md          ← nội dung giai đoạn 1
├── stage2.md          ← nội dung giai đoạn 2
└── learning-approach.md  ← file này
```
