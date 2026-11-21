# Danh sách lớp v0 — VuVanThang · G11

Viết tiếp từ slide 54 bài giảng. Mỗi lớp trả lời: **model cần nhận ra cái này để làm gì?**

## DMS (ít nhất 6 lớp)

| # | Lớp / thuộc tính | Giá trị | Để làm gì (chức năng DMS) | Nhìn thấy được trên IR? |
|---|---|---|---|---|
| 1 | `eye_left` / `eye_right` | `open` / `closed` / `unknown` | Phát hiện buồn ngủ (PERCLOS, nhắm mắt kéo dài) và mất tập trung | Có, trừ khi bị phản xạ LED kính, kính râm tối hoặc tay/tóc che |
| 2 | `gaze_zone` | `road` / `mirror_l` / `mirror_r` / `mirror_c` / `cluster` / `center` / `lap` / `other` | Theo dõi hướng nhìn để cảnh báo rời mắt khỏi đường quá lâu | Có (qua hướng đầu và đồng tử trên NIR), giảm độ chính xác khi đeo kính râm hoặc quay đầu mạnh |
| 3 | `glasses` | `none` / `clear` / `sun` / `unknown` | Đánh giá độ tin cậy nhánh theo dõi mắt/gaze và xử lý chói kính | Gọng kính thấy rõ, nhưng trên IR kính râm có thể trông gần như trong suốt nên dễ nhầm với kính thường |
| 4 | `face13` | 13 điểm mốc mặt + cờ `occluded` / `outside` | Tính độ mở mắt (EAR), ngáp (MAR) và ước lượng tư thế đầu (yaw/pitch/roll) | Có với mặt rộng ≥ 60 px; điểm bị che phải gắn cờ `occluded` hoặc `outside` |
| 5 | `hand` | `whose` (`driver` / `passenger`), `side` (`left` / `right`), `on_wheel` (`yes` / `no` / `unknown`) | Phát hiện tay trên/rời vô lăng (hands-on-wheel) để kiểm tra mức độ sẵn sàng tiếp quản lái ở xe ADAS/tự lái | Có qua hình dạng và vị trí, khó hơn ở góc khuất tối |
| 6 | `phone` | `state`: `held` / `mounted` / `lap` | Cảnh báo mất tập trung nghiêm trọng khi tài xế cầm hoặc sử dụng điện thoại | Có nếu lộ cạnh, góc hoặc màn hình; khó phân biệt nếu vật tối không rõ hình dạng |
| 7 | `person.seatbelt` | `worn_correct` / `worn_incorrect` / `not_worn` / `unknown` | Cảnh báo tài xế chưa thắt hoặc thắt sai dây an toàn (dưới nách, sau lưng) | Có khi dây và áo đủ tương phản độ sáng, khó thấy khi áo tối trùng độ sáng dây |

## OMS (ít nhất 6 lớp)

| # | Lớp / thuộc tính | Giá trị | Để làm gì (chức năng OMS) | Nhìn thấy được trên IR? |
|---|---|---|---|---|
| 1 | `person` | `seat`: `driver` / `front_passenger` / `rear_left` / `rear_middle` / `rear_right` | Đếm hành khách và xác định chính xác từng vị trí ghế theo xe để kích hoạt cảnh báo an toàn và điều khiển túi khí | Có tốt nhờ hình dáng cơ thể (nhưng mất màu sắc) |
| 2 | `person.seatbelt` | `worn_correct` / `worn_incorrect` / `not_worn` / `unknown` | Kiểm tra tình trạng thắt dây an toàn hàng ghế sau | Phụ thuộc hoàn toàn vào chênh lệch độ sáng giữa dây và áo vì IR không có màu |
| 3 | `child_seat` | `seat`, `occupied`: `no` / `unknown` | Nhận diện ghế trẻ em theo từng vị trí ghế để quản lý an toàn và nhắc kiểm tra trẻ | Khung ghế thấy rõ trên IR, nhưng lòng ghế sâu tối dễ phải ghi `occupied = unknown` |
| 4 | `infant_seat` | `seat`, `facing`: `rear` / `front` | Phát hiện nôi em bé và hướng lắp đặt (quay ngược/xuôi) để ngắt túi khí ghế phụ khi quay ngược và cảnh báo chống bỏ quên trẻ (CPD) | Có, thấy rõ qua hình khối nôi |
| 5 | `object` | `seat`, `type`: `bag` / `box` / `cup` / `other` | Phân biệt hành lý/đồ vật đặt trên ghế với người thật để tránh báo nhầm nhắc thắt dây an toàn hoặc bung túi khí sai | Thấy hình dáng tổng thể trên IR nhưng mất màu sắc và chất liệu nên dễ phải gán `other` |
| 6 | `frame_meta` | `modality`: `rgb` / `ir`, `light`: `day` / `night` / `unknown`, `camera_blocked`: `yes` / `no` | Kiểm soát chất lượng ảnh đầu vào và phát hiện camera cabin bị che khuất để cảnh báo hệ thống tạm mất tác dụng | `camera_blocked` thấy trực tiếp trên cả RGB và IR; riêng ảnh dựng SVIRO ghi `light = unknown` |

## 2 câu hỏi leo thang

Câu hỏi bạn muốn hỏi người viết guideline (hoặc khách hàng) vì guideline v1.0 chưa trả lời. Mỗi câu kèm **tên khung** làm ví dụ.

1. Câu hỏi: Khi trên ảnh SVIRO xuất hiện người kích thước nhỏ ngồi lọt trong vùng `child_seat` hoặc `infant_seat`, trong khi tài liệu mô tả bộ ảnh không có trẻ em ngồi và schema `child_seat` chỉ có `occupied = no / unknown` (không có `yes`), thì nên vẽ cả hai box (`person` + `child_seat`/`infant_seat`) hay chỉ vẽ ghế và bỏ `person`?
   Khung ví dụ: `S04_rgb` (ghế `rear_right`, box `child_seat` tại `x=[11,398]` trùng vùng với box `person` tại `x=[33,320]`; trường hợp tương tự gặp ở `S02_rgb`, `S03_rgb`, `S07_rgb`).
   Bạn tạm gán thế nào và vì sao: Tạm thời vẽ tách cả hai box (`child_seat` để `occupied = unknown`, `person` kèm `face` và `seatbelt = not_worn`) ở cùng vị trí `seat` theo xe để không bỏ sót người trong khung hình, đồng thời đánh dấu ca này để hỏi coach xác nhận trước khi chốt bản sửa sau review.
2. Câu hỏi: Khi khuôn mặt tài xế bị nghiêng hoặc che khuất phần lớn làm nhiều điểm mốc `face13` không thể quan sát (phải bật cờ `outside` cho 8–12/13 điểm), ngưỡng tối thiểu nào thì vẫn tiếp tục vẽ `face` + `face13`, và khi nào được coi là "bị che gần hết, không thấy cả mắt lẫn mũi" để không vẽ cả box `face` lẫn `face13` theo mục 4.5?
   Khung ví dụ: `D05` (box `face` rộng `164 px` tại `x=[896,1061]` nhưng 12/13 điểm `face13` phải bật `outside = 1`, chỉ giữ `eye_left = unknown`, `eye_right = unknown`, `gaze_zone = other`; tương tự ở `D03` với 9/13 điểm bật `outside = 1`).
   Bạn tạm gán thế nào và vì sao: Tạm thời vẫn vẽ box `face` (đặt `eye_left/eye_right = unknown`, `gaze_zone = other`) và giữ skeleton `face13` nhưng bật `outside = 1` cho mọi điểm không nhìn thấy thay vì đoán tọa độ, tuân thủ nguyên tắc không đoán ở mục 4.4 và 4.6 trong lúc chờ làm rõ ngưỡng bỏ `face`.
