# Báo cáo Reflection - Lab 18 Lakehouse

**Họ và tên:** Phạm Lê Hoàng Nam  
**MSHV:** 2A202600416

---

## Phản tư về Anti-pattern

Dựa trên các nội dung thực hành và lý thuyết, anti-pattern mà hệ thống dữ liệu của nhóm tôi dễ gặp rủi ro nhất là **"Small File Problem" (Vấn đề tệp tin quá nhỏ)**.

**Lý do:**
Trong bối cảnh hệ thống thực tế thường xuyên tiếp nhận dữ liệu streaming hoặc các lô (batch) nhỏ liên tục (ví dụ: log API từ người dùng, telemtry data), nếu ghi thẳng vào Data Lakehouse mà không có cơ chế quản lý, hệ thống sẽ nhanh chóng sinh ra hàng ngàn tệp dữ liệu kích thước cực nhỏ. Hậu quả là thời gian truy vấn bị kéo dài đáng kể do chi phí I/O (overhead) của việc đọc metadata và mở tệp, tương tự như vấn đề đã gặp phải trong Notebook `02_optimize_zorder`.

**Hướng khắc phục:**
Để giảm thiểu rủi ro này, chúng tôi sẽ cần thiết lập lịch trình bảo trì dữ liệu định kỳ bằng tính năng `OPTIMIZE` (nhằm gom gộp / compact các tệp nhỏ) kết hợp với `Z-ORDER` theo các trường thường xuyên được truy vấn. Việc tận dụng tính năng tự động tối ưu hóa (auto-compaction) của Delta Lake cũng sẽ giúp duy trì hiệu năng ổn định về dài hạn mà không đòi hỏi thao tác thủ công.

---

## Các minh chứng (Evidence)

Dưới đây là các ảnh màn hình minh chứng cho các bài thực hành đã thực hiện thành công:

- **NB1 - Delta Basics:** [01_delta_basics_evd.png](./01_delta_basics_evd.png)
- **NB2 - Optimize & Z-Order:** [02_optimize_zorder_evd.png](./02_optimize_zorder_evd.png)
- **NB3 - Time Travel:** [03_time_travel_evd.png](./03_time_travel_evd.png)
- **NB4 - Medallion Pipeline:** [04_medallion_evd.png](./04_medallion_evd.png)
- **Minh chứng Delta Log on disk:** [delta_log_evd.png](./delta_log_evd.png)
