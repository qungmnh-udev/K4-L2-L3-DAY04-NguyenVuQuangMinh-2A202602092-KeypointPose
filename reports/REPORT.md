# Báo cáo Ngày 4 - Keypoint & Pose

Họ tên: Nguyễn Vũ Quang Minh   Nhóm: Không có tên   Ngày: 16/09/2026

> Cách dùng: copy file này thành `reports/REPORT.md`. Điền bằng số liệu do công cụ sinh ra;
> không tự ước lượng hoặc sửa số trong file JSON.

## 1. Nhãn của tôi

<!-- Lấy số từ reports/visibility_report.md hoặc outputs/visibility_report.json sau Chặng 4.
Số ảnh phải là 20; số skeleton là tổng số người trong 20 ảnh. Thời gian trung bình = tổng
thời gian gán / 20. -->

| # | Khớp | v=2 | v=1 | v=0 | %v=1 |
| ---: | --- | ---: | ---: | ---: | ---: |
| 0 | nose | 24 | 1 | 4 | 3% |
| 1 | left_eye | 20 | 3 | 6 | 10% |
| 2 | right_eye | 23 | 1 | 5 | 3% |
| 3 | left_ear | 13 | 12 | 4 | 41% |
| 4 | right_ear | 21 | 6 | 2 | 21% |
| 5 | left_shoulder | 25 | 4 | 0 | 14% |
| 6 | right_shoulder | 28 | 1 | 0 | 3% |
| 7 | left_elbow | 24 | 4 | 1 | 14% |
| 8 | right_elbow | 24 | 5 | 0 | 17% |
| 9 | left_wrist | 19 | 8 | 2 | 28% |
| 10 | right_wrist | 19 | 7 | 3 | 24% |
| 11 | left_hip | 18 | 10 | 1 | 34% |
| 12 | right_hip | 22 | 6 | 1 | 21% |
| 13 | left_knee | 17 | 5 | 7 | 17% |
| 14 | right_knee | 16 | 6 | 7 | 21% |
| 15 | left_ankle | 14 | 6 | 9 | 21% |
| 16 | right_ankle | 14 | 6 | 9 | 21% |

| Chỉ số | Giá trị |
| --- | ---: |
| Số ảnh đã gán | 20 |
| Số skeleton | 29 |
| v=2 / v=1 / v=0 |  |
| Thời gian trung bình mỗi ảnh | 4 |

Ba khớp có `%v=1` cao nhất (chép từ `reports/visibility_report.md`):

1. left_ear
2. left_hip
3. left_wrist

Chúng có đúng là những khớp bạn thấy khó gán nhất không? Nếu không, giải thích.

`Vì khớp nhỏ và dễ bị che khuất sau vật thể khác nên đúng, là những vật thể này khá là khó gán.`

## 2. Chấm với gold

<!-- Lấy hai cột từ outputs/eval_vs_gold.json: một lần ngay khi protected release mở và một
lần sau rework. Đếm số phần tử trong từng danh sách lỗi, không tự làm tròn. -->

| Chỉ số | Trước rework | Sau rework |
| --- | ---: | ---: |
| OKS trung bình | 0.928 | |
| OKS@0.50 | 1.000 | |
| OKS@0.75 | 1.000 | |
| Lỗi `dao_trai_phai` | 1 | |
| Lỗi `nham_nguoi` | 0 | |
| Lỗi `xoa_khop_bi_che` | 3 | |

**Tôi đã sửa gì giữa hai lần chạy** (ghi cụ thể: ảnh nào, người thứ mấy, khớp nào):

`Không sửa lại ảnh nào. Bức đảo trái/phải được xác định tại ảnh train_13.jpg là model sai, không phải người sai.`

**Lỗi đảo trái/phải của tôi xảy ra ở ảnh nào?** Ảnh đó dễ hay khó? Nếu là ảnh dễ,
bạn nghĩ vì sao mình vẫn sai?

`Không có lỗi đảo trái/phải trong bài.`

## 4. Model

<!-- Chép số từ outputs/eval_model.json sau Chặng 6. “Chênh” = sau fine-tune trừ baseline;
đây là quan sát trên tập test, không phải chất lượng sản phẩm. -->

| Chỉ số | yolo26n-pose gốc | Sau fine-tune | Chênh |
| --- | ---: | ---: | ---: |
pose_mAP50          0.8450          0.8450   +0.0000
pose_mAP50_95       0.6853          0.6908   +0.0055
pose_precision      0.9734          0.9792   +0.0058
pose_recall         0.8462          0.8462   +0.0000
box_mAP50           0.9785          0.9600   -0.0185
box_mAP50_95        0.8119          0.8041   -0.0078

### Trả lời năm câu hỏi ở cuối notebook

> Mỗi câu cần trỏ tới ảnh/chỉ số cụ thể. Một con số thấp không tự chứng minh nhãn sai;
> kiểm lại bằng bằng chứng thị giác và kết quả gold.

1. `pose_mAP50-95` thay đổi bao nhiêu? Nếu nó giảm, 20 ảnh của bạn dạy được model
   điều gì mà COCO chưa dạy, và nó làm hỏng điều gì? `Đã tăng thêm 0.0055`

2. `box_mAP` và `pose_mAP` chênh nhau bao nhiêu? Model tìm *người* dễ hơn hay tìm
   *khớp* dễ hơn? Vì sao? `Chênh nhau tới tận 0.115, có nghĩa là model này sẽ tìm người dễ hơn là tìm khớp tại vì mô hình box chỉ là mô hình 2D, không có chiều sâu.`

3. Một ảnh test model đoán sai - gọi tên lỗi theo bốn loại của slide 43
   (lệch nhẹ / đảo trái/phải / nhầm người / trượt hẳn): `test_02, ảnh này model đoán nhầm người.`

4. Ảnh nào có OKS thấp nhất giữa nhãn của bạn và model? Ai đúng, và bạn dựa vào đâu?
   `Ảnh train_15 với điểm số là 0.576. Nhãn của tôi đúng hơn, vì tôi đã có phán đoán và dựa vào hình dáng, kiểu đứng và các hint xoay quanh môi trường để có thể đánh giá chính xác hơn về chủ thể.`


5. Ảnh bạn gán tệ nhất có *cũng* là ảnh model đoán tệ nhất không? Nếu có, điều đó
   nói gì về bức ảnh đó?

   `Nếu như có một bức mà tôi gán tệ và model cũng gán tệ, thì có thể là do nguồn data thật sự không đủ chắc chắn để sử dụng. Chủ thể và ảnh có thể rất mờ hoặc bị che khuất quá khả năng xác nhận.`

## 5. Một rule evidence bạn đã dùng

Chọn một keypoint trong ảnh core mà bạn phải quyết định giữa `v=1` và `v=0`. Nêu ảnh, người,
khớp, bằng chứng nhìn thấy và lý do chọn trạng thái đó trong 3-5 câu.

`train_04, PERSON 91 + LEFT_HIP. Tại keypoint này thì người đang ngồi trên xe máy, phần hông trái bị che bởi phần bình xăng của xe, và người cũng mặc quần áo dài. Tôi đã chọn khớp còn trong khung (v=1) vì căn cứ vào vị trí của RIGHT_HIP và tư thế ngồi của người, có thể xác định được vị trí gần chính xác của LEFT_HIP.`

<!-- Cấu trúc gợi ý: (1) train_XX + người thứ mấy + keypoint; (2) căn cứ thị giác như phần cơ
thể liền kề, trang phục hoặc vật che; (3) vì sao khớp còn trong khung (v=1) hay đã ra khỏi
khung (v=0). -->
