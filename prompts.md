# Khai báo dùng AI — VuVanThang

Không dùng AI thì xóa bảng và ghi: **Không dùng.**

Nhắc lại: không đưa ảnh cabin hay ảnh tài xế (kể cả ảnh chụp màn hình) cho bất kỳ công cụ AI nào.

| # | Công cụ | Bạn hỏi gì (tóm tắt) | Bạn dùng kết quả thế nào | Bạn có kiểm lại không? Bằng gì? |
|---|---|---|---|---|
| 1 | Antigravity | Đọc guideline repo, khởi tạo repo GitHub nộp bài, viết script đẩy 22 ảnh cốt lõi + 4 ảnh mở rộng lên CVAT local (`Day12-G11`, `Day12-G11-extra`), tạo task mẫu sơ đồ hình học giả lập (`Day12-Demo-Synthetic`) và biên soạn sổ tay `CVAT_DRAWING_GUIDE.md` | Dựng sẵn repo và workspace CVAT local để tự gán nhãn thủ công (không gửi ảnh cabin cho AI, không nhờ AI tự gán nhãn) | Có, đối chiếu trực tiếp với `GUIDE.md`, `RULES.md`, `SUBMISSION.md`, `labels_cabin_v1.json` và kiểm tra đủ frame trên CVAT local |
| 2 | Antigravity | Chạy cục bộ `scripts/rgb_ir_diff.py`, hỗ trợ export XML (`save_images=false`) thành `export_final.zip` / `export_extra.zip` sau khi tự gán nhãn xong, chạy `scripts/cabin_check.py` và rà soát lỗi cấu trúc XML | Xuất các file log `check_cpA.txt`, `check_cpB.txt`, `check_final.txt`, `check_extra.txt` và phát hiện các điểm cần tự kiểm tra lại trên CVAT | Có, kiểm tra `python3 -m zipfile -l` chỉ chứa `annotations.xml` và đối chiếu kết quả `cabin_check.py` (`0 lỗi, 0 cảnh báo`) |
| 3 | Antigravity | Góp ý câu chữ và cấu trúc cho `class_list_v0.md`, `ir_log.csv`, `self_review.md` và `TEAM.md` dựa trên XML export và số liệu `ir_pairs/ir_pairs.csv` | Hoàn thiện câu chữ các bảng báo cáo và câu hỏi leo thang; giữ nguyên `exit_ticket.md` tự làm và phần review chéo chờ đối chiếu với bạn | Có, tự rà lại từng dòng mô tả đối chiếu với XML, số liệu `ir_pairs.csv` và bài làm thực tế trên CVAT |
