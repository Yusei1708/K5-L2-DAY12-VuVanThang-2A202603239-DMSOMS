# Tự đánh giá — VuVanThang · G11

## 1. Tự rà soát 4 cảnh

Nghỉ tay 1 phút, mở lại bài của mình, chọn 4 ảnh (2 S##, 2 D##), đọc như người lạ.

Loại lỗi: `seat_flip`, `wrong_class`, `guess`, `box`, `missing`, `extra`, `lr_flip` (trái/phải mặt), `flag` (thiếu cờ điểm), `ok`.

| Ảnh | Đối tượng | Loại lỗi | Ghi chú | Đã sửa? |
|---|---|---|---|---|
| `S07_rgb` / `S07_ir` | `person`, `child_seat` (`rear_right`, `rear_left`) | `ok` | `S07_rgb` rất tối nên giữ `seatbelt=unknown` và `occupied=unknown`; sang `S07_ir` sáng hơn mới cập nhật `not_worn` và `occupied=no`, không đoán ngược từ RGB. | Đã kiểm tra đạt (riêng cặp `person` + `child_seat` ở `rear_right` chờ hỏi coach) |
| `S04_rgb` | `child_seat`, `person` (`rear_right`) | `wrong_class` | Vị trí ghế theo xe đúng (`trái ảnh = rear_right`), nhưng đang vẽ đồng thời `child_seat` (`x=[11,398]`) và `person` (`x=[33,320]`) chồng lên nhau — cần xác nhận lại quy tắc SVIRO (bộ ảnh không có trẻ em ngồi, `child_seat` không có `occupied=yes`). | Đã ghi vào câu hỏi leo thang & `TEAM.md` để hỏi coach |
| `D04` | `face`, `face13`, `phone` | `box` / `lr_flip` | `phone` (`state=held`) và `face` ôm gọn, 8 điểm mắt đã bật `outside=1` khớp `eye_left=eye_right=unknown`. Tuy nhiên tọa độ `left_eye_outer=(1155.8,502.7)` và `right_eye_outer=(1016.9,502.7)` đang nằm ngang cùng `y` (theo khung template đứng) thay vì xoay trục mắt theo chiều dọc `y` (`left_eye_*` có `y` lớn hơn) trên ảnh xoay 90°. | Cần mở CVAT xoay chỉnh lại các điểm `face13` và kiểm tra phân biệt `occluded` vs `outside` |
| `D05` | `face13` | `flag` / `box` | 12/13 điểm `face13` đang bật `outside=1` và hai điểm mắt đang có cùng `y=395.2` (giống template mặc định chưa xoay 90°). Cần kiểm lại trên ảnh gốc xem điểm nào thực sự bị che trong khung hình (nên dùng `occluded` nếu đoán chắc vị trí) và chỉnh lại trục mắt `y` cho đúng hướng đầu tài xế. | Cần mở CVAT kiểm tra và chỉnh lại trước khi export bản cuối |

## 2. Lỗi mình lặp lại nhiều nhất hôm nay

Lỗi: Chưa xoay hết khung template `face13` sang trục dọc (`y` của `left_eye_*` lớn hơn `right_eye_*`) ở các khung DMS có vùng mắt bị che khuất (`D03`, `D04`, `D05`), và lúng túng khi gặp ghế SVIRO có đồng thời `person` và `child_seat`/`infant_seat` (`S02`, `S03`, `S04`, `S07`).

Vì sao mình mắc: Khi mắt bị che không thấy trực tiếp và bật cờ `outside`, dễ giữ nguyên bố cục nằm ngang mặc định của template CVAT (chỉ `D01` và `D06` có mắt rõ nên mới kéo đúng chiều xoay 90°); đồng thời `cabin_check.py` chỉ kiểm trục `x` nên không cảnh báo lỗi giữ nguyên template đứng trên `D##`.

Lần sau tránh bằng cách: Luôn kiểm tra bất biến `y(left_eye_outer) > y(right_eye_outer)` trên mọi ảnh `D##` trước khi rời khung, phân biệt rõ `occluded` (che nhưng ước lượng chắc vị trí trong ảnh) với `outside` (ngoài khung hoặc không thể ước lượng), và kiểm kỹ mục 3 & 4.8 trước khi vẽ `person` chồng trong `child_seat`.

## 3. Ghi chú về disagreement_log

Một dòng bạn của mình đúng và mình sai. Mình học được gì từ dòng đó: *[Chờ điền sau khi đổi `peer_export_final.zip` và chạy `cabin_check.py diff` với bạn cùng mã G11.]*
