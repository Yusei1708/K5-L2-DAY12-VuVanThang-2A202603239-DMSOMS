# TEAM — Ngày 12

Mã bộ ảnh: **G11** · Hình thức: nhóm / cá nhân (xóa một)

| Họ tên | MSSV | Đã gán đủ 22 ảnh? | Review chéo với (peer diff) | Số dòng disagreement đã xử lý |
|---|---|---|---|---|
| VuVanThang | 2A202603239 | có | | |
| | | | | |

Ca hai bạn vẫn không thống nhất và đã hỏi coach (ghi tên ảnh + đối tượng):

- `S02_rgb`/`S02_ir` (`rear_right`), `S03_rgb`/`S03_ir` (`rear_middle`), `S04_rgb`/`S04_ir` (`rear_right`), `S07_rgb`/`S07_ir` (`rear_right`): xuất hiện đồng thời `person` và `child_seat`/`infant_seat` trên cùng vị trí ghế — cần đối chiếu và hỏi coach theo mục 3 & 4.8.
- `D03`, `D04`, `D05` (`face13`): cách đặt và bật cờ `outside=1` / `occluded=1` khi có 8–12/13 điểm mặt bị che khuất trên ảnh NIR xoay ~90°.

## Cá nhân

Mỗi người một mục, 1–2 dòng mỗi ý.

### VuVanThang-2A202603239
- Phần mình làm (ngoài 22 ảnh của mình, ví dụ ai review ai, ai tổng hợp ca hỏi coach): Đã hoàn thành gán nhãn đủ 22 ảnh cốt lõi (`Day12-G11`) và 4 ảnh mở rộng (`Day12-G11-extra`: `S09–S10`), export `export_final.zip` và `export_extra.zip` (không kèm ảnh), chạy tự kiểm `cabin_check.py` đạt `0 lỗi, 0 cảnh báo`, đồng thời tổng hợp `ir_log.csv` và `class_list_v0.md`. Đang chờ đổi file với bạn cùng mã `G11` để hoàn thiện `disagreement_log.csv`.
- Điều mình học được hôm nay: Cách gọi vị trí ghế theo xe khi camera OMS nhìn ngược chiều (`trái ảnh = rear_right`), cách đối chiếu ảnh RGB tối với IR-sim và NIR thật để giữ `unknown` thay vì đoán, và cách xoay skeleton `face13` đúng trục trên ảnh DMS xoay ~90° (`left_eye_*` có `y` lớn hơn `right_eye_*`).

Làm cá nhân: chỉ cần điền dòng của mình, tên bạn được coach ghép, và mục "Cá nhân" của mình.
