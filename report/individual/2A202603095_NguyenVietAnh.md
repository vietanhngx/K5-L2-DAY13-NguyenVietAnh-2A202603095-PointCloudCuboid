# Báo cáo Day 13 — Nguyễn Việt Anh, 2A202603095

## 1. Kết luận trước
- MSSV: 2A202603095
- Task: `D13-2A202603095`
- Frame: `s01f11`, `s01f59`, `s02f04`, `s02f51`, `s03f15`, `s03f56`, `s04f03`, `s04f45`, `s05f03`, `s05f32`
- Hệ điều hành: Windows, chạy lệnh trong WSL
- Trình duyệt: Chrome
- Phiên bản CVAT: 2.81.0
- MSSV bạn ngồi cạnh: 2A202603243

Tổng cộng có 92 cuboid, trong đó frame `s02f51` ít nhất với 5 cuboid vì các vật còn lại thưa điểm hoặc bị che, khó xác định. `selfcheck` gắn cờ 13/92 cuboid. Em tự tin nhất khi chọn đúng loại vật, còn đáy hộp là phần em chưa chắc vì kiểm tra bằng Side/Front thấy hộp sát đường nhưng `selfcheck` vẫn gắn cờ.

- Xoay khung 3D bằng chuột trái để nhìn từ trên xuống, kéo chuột phải để dời góc nhìn.
- Chọn label và Shape, nháy đúp để đặt hộp, chỉnh dài, rộng và hướng ở Top, chỉnh đáy và chiều cao ở Side/Front.
- Bật Cuboid orientation trong Appearance, dùng `python scripts/lab.py preview` để xem cuboid trên ảnh camera.

## 2. Đọc frame (Bước 2)
Hai frame đọc kỹ. Ảnh: `t1-<frame>-3d`, `t1-<frame>-top` (ảnh nhìn từ trên xuống, có đánh dấu phía trước, +x, +y).

| Frame | Phía trước xe ở cạnh nào của ảnh nhìn từ trên xuống; +x, +y chỉ về đâu | Mặt đường (phẳng/dốc; thấp hơn LiDAR ~ m) | Vật xa nhất đọc được (~ m) | Loại vật thấy | Vật phía sau / hai bên | Cụm chưa đọc được |
|---|---|---|---|---|---|---|
| `s01f11` | Phía trước (−y) ở cạnh trái ảnh. +x hướng xuống, +y hướng sang phải. | Khá phẳng. Chưa đo độ thấp so với LiDAR. | 38,59 m (hộp xa nhất đã gán) | Car, Bus | Một xe con bên trái | Thấy vật trên ảnh camera nhưng chưa thấy cụm điểm LiDAR tương ứng. |
| `s01f59` | Phía trước (−y) ở cạnh trái ảnh. +x hướng xuống, +y hướng sang phải. | Khá phẳng. Chưa đo độ thấp so với LiDAR. | 26,68 m (hộp xa nhất đã gán) | Car, Cyclist, Pedestrian | Một xe con ở hông trái. | Thấy vật trên ảnh camera nhưng chưa thấy cụm điểm LiDAR tương ứng. |

Camera: Thấy một xe ô tô màu xanh. Ở `s01f59`, thấy đèn đỏ trên một xe và áo màu vàng của người đi xe máy.

LiDAR: Ở `s01f11`, tâm hộp xe con #1 cách LiDAR 6,39 m theo phương ngang. Hộp xe dài 4,432 m, rộng 1,732 m và cao 1,453 m. Đáy hộp thấp hơn LiDAR khoảng 1,72 m.


Tám frame lướt:

| Frame | Phía trước ở cạnh nào của ảnh nhìn từ trên xuống | Điều lạ (nếu có) |
|---|---|---|
| `s02f04` | Cạnh trái nếu giữ cùng góc Top như hai frame đầu | Chưa ghi nhận. |
| `s02f51` | Cạnh trái nếu giữ cùng góc Top như hai frame đầu | Một số vật thưa điểm hoặc bị che, khó xác định. |
| `s03f15` | Cạnh trái nếu giữ cùng góc Top như hai frame đầu | Chưa ghi nhận. |
| `s03f56` | Cạnh trái nếu giữ cùng góc Top như hai frame đầu | Chưa ghi nhận. |
| `s04f03` | Cạnh trái nếu giữ cùng góc Top như hai frame đầu | Chưa ghi nhận. |
| `s04f45` | Cạnh trái nếu giữ cùng góc Top như hai frame đầu | Chưa ghi nhận. |
| `s05f03` | Cạnh trái nếu giữ cùng góc Top như hai frame đầu | Chưa ghi nhận. |
| `s05f32` | Cạnh trái nếu giữ cùng góc Top như hai frame đầu | Chưa ghi nhận. |

## 3. Ba cuboid có hướng dẫn (Bước 3)
Ảnh: `t2-a-car`, `t2-b-bus-truck`, `t2-c-ped-cyclist`.

| Vật | Frame, idx | Class | Mũi tên yaw đúng hướng? | Đáy chạm đường? | Đã kiểm Top/Side/Front + ảnh? | Kích thước trong bảng 4.3? |
|---|---|---|---|---|---|---|
| (a) xe con gần | `s01f11` #1 | Car | 268,6°. Vạch đỏ dọc thân, hướng về phía trước của cảnh. | Đáy −1,716 m, gần mốc −1,7 m | Ảnh có camera và ba view. Hộp hiện rõ ở khung 3D lớn. | 4,432 × 1,732 × 1,453 m, trong dải sedan/hatchback |
| (b) xe buýt hoặc xe tải | `s01f11` #3 | Bus | Vạch đỏ dọc thân, chưa rõ đầu xe tương ứng | Chưa thấy rõ mặt đường ngay dưới hộp | Ảnh có Top, Side, Front và camera. | 12,152 × 2,781 × 3,282 m, rộng hơn dải xe buýt thường |
| (c) người đi bộ hoặc xe hai bánh | `s01f59` #9 | Cyclist | 265,6°. Vạch đỏ dọc thân, chưa rõ đầu xe tương ứng | Đã chỉnh và kiểm lại bằng Side/Front | Ảnh có Top, Side, Front và camera. | 1,909 × 0,659 × 1,512 m. Rộng dưới mốc 0,70 m tham khảo cho xe máy |

Chưa ghi nhận kết quả vòng hiệu chuẩn. Ở `s02f51`, em bỏ qua các vật bị che hoặc có quá ít điểm để xác định (LOW_VISIBILITY).

## 4. Bốn hiện tượng chất lượng (Bước 4)
Nguyên nhân ghi là **giả thuyết**. Frame mượn thì ghi rõ nguồn: task chung, hoặc MSSV của bạn cho mượn.

| Hiện tượng | Frame / tên ảnh / nguồn | Nguyên nhân (giả thuyết) | Thấy gì trên point cloud | Hệ quả khi gán (theo guideline) |
|---|---|---|---|---|
| 1. Bóng che | `s01f11` / `t4-1-bong-che.png` / task cá nhân | Vật phía trước chắn tia LiDAR | Có một vùng ít điểm cạnh cụm điểm dày. Ranh giới vùng bị che chưa rõ trong ảnh. | Bật occluded và bao phần bị che có thể suy ra. |
| 2. Thưa ở xa | `s01f11` / `t4-2-thua-o-xa.png` / task cá nhân | Ở xa, các điểm lấy mẫu cách nhau hơn | Quét gần có các vệt điểm dày hơn. Những cụm ở xa hiện thưa và rời hơn. | Dùng mốc 15–20% thể tích. Nếu quá thưa và thiếu căn cứ, bỏ qua và ghi LOW_VISIBILITY. |
| 3. Phản xạ thấp / ít điểm bất thường | `s01f11` / `t4-3-phan-xa-thap.png` / task cá nhân | Bề mặt có thể phản xạ yếu | Một số vật thấy trên camera nhưng ít điểm trên LiDAR. Ảnh chưa đủ để xác định nguyên nhân. | Đối chiếu camera và chỉ gán khi đủ căn cứ. |
| 4. Điểm lạ | `s01f11` / `t4-4-diem-la.png` / task cá nhân | Có thể là nhiễu hoặc phản xạ gián tiếp | Có các điểm rời, không tạo hình vật rõ ràng. | Bỏ qua điểm rời không đủ căn cứ tạo thành vật. |

## 5. Số từ cuboid của tôi (Bước 5)
Lấy từ `submission/D13-<MÃ SỐ>-bangA.csv`; số điểm lấy từ cột `points_in` của `-tukiem.csv`. Mặt đường khoảng −1,7 m: z đáy lệch hơn 0,3 m là lơ lửng hoặc chìm.

| Cuboid (frame, idx, class) | z | h | z đáy = z − h/2 | dist_m | r = √(x²+y²+z²) | yaw_deg | Nhận xét (lơ lửng / chìm / chạm đường) |
|---|---|---|---|---|---|---|---|
| `s01f11` #1 Car | -0.990 m | 1.453 m | -1.716 m | 6.39 m | 6.47 m | 268.6° | Gần cao độ mặt đường tham chiếu |
| `s01f59` #2 Car | -0.958 m | 1.506 m | -1.711 m | 2.95 m | 3.10 m | 268.0° | Gần cao độ mặt đường tham chiếu |
| `s04f03` #15 Car | 0.212 m | 1.766 m | -0.671 m | 49.14 m | 49.14 m | 101.3° | Cao hơn mốc −1,7 m, cần xem mặt đường tại vị trí xe |

(a) Gần nhất: `s01f59` #2 Car, `dist_m` 2.95 m, r 3.10 m. Xa nhất: `s04f03` #15 Car, `dist_m` 49.14 m, r 49.14 m.

(b) Car #1 (`s01f11`) có kích thước 4,432 × 1,732 × 1,453 m, trong khoảng sedan/hatchback. Cyclist #9 (`s01f59`) có kích thước 1,909 × 0,659 × 1,512 m. Nếu là xe đạp, chiều dài cao hơn mốc 1,90 m khoảng 0,01 m và chiều cao thấp hơn mốc 1,60 m. Nếu là xe máy, chiều rộng nhỏ hơn mốc 0,70 m. Cần xác nhận loại xe trên ảnh để chọn khoảng so sánh.

(c) Car #1 (`s01f11`) có yaw 268,6°, gần 270°. Car #15 (`s04f03`) có yaw 101,3°, cách 90° khoảng 11,3°. Cần đối chiếu đầu xe trên camera để xác nhận hướng thực tế.

Mật độ theo khoảng cách (4 cuboid ở các `dist_m` khác nhau):

| Cuboid (frame, idx, class) | dist_m | Số điểm (`points_in`) |
|---|---|---|
| `s01f11` #1 Car | 6.4 m | 2142 |
| `s05f03` #10 Car | 15.0 m | 375 |
| `s01f11` #9 Car | 25.0 m | 197 |
| `s01f11` #6 Car | 35.1 m | 41 |

Trong bốn mẫu, khi khoảng cách tăng từ 6,4 m lên 35,1 m (khoảng 5,5 lần), số điểm giảm từ 2.142 xuống 41, còn khoảng 2%. Xu hướng giảm giống slide. Hai mẫu ở 15,0 m và 35,1 m có ít điểm hơn trung vị của nhóm tương ứng trên slide.

Tự kiểm: Em chỉnh lại đáy Car #7 (`s01f11`) và chỉnh hướng, kích thước, đáy Cyclist #9 (`s01f59`). Em giữ hai hộp sau khi đối chiếu Side/Front và thấy đáy sát mặt đường tại vị trí vật.

Các cờ còn lại cần đối chiếu trực tiếp trong CVAT.

Đổi kiểm: Bạn 2A202603243 đã đối chiếu chiều cao 1,453 m và yaw 268,6° của Car #1 (`s01f11`). Cả hai số đều khớp với Bảng A.

## 6. Ý nghĩa của nhãn, cách dùng AI và điều còn chưa chắc
Nhãn đo bằng mét để mô hình học vị trí và kích thước vật trong không gian 3D. Vật thưa điểm cần quy tắc riêng vì ít điểm làm hình dạng và kích thước khó xác định, dễ tạo nhãn thiếu chính xác.

- Công cụ: Codex.
- Câu hỏi: Cách chạy CVAT, xuất kết quả bằng `sync` và kiểm tra cuboid bị gắn cờ.
- Cách sử dụng: Em tự chạy lệnh, đối chiếu point cloud trong CVAT và chỉnh các hộp đã kiểm. Codex hỗ trợ sắp xếp thông tin đã trao đổi vào mẫu báo cáo.

Em còn chưa chắc về chênh lệch giữa mặt đường mà `selfcheck` ước tính và mặt đường em thấy khi kiểm tra đáy hộp.
