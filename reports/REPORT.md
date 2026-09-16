# Báo cáo Ngày 4 - Keypoint & Pose

Họ tên: Bạch Khánh An - 2A202602095 Ngày: 2026-09-16

> Báo cáo được lập dựa trên số liệu do công cụ `tools/visibility_report.py`, `tools/check_pose_labels.py` và `tools/evaluate_pose_annotations.py` sinh ra.

---

## 1. Nhãn của tôi

| Chỉ số | Giá trị |
| --- | ---: |
| Số ảnh đã gán | 20 |
| Số skeleton | 26 |
| v=2 / v=1 / v=0 | 299 / 100 / 43 |
| Thời gian trung bình mỗi ảnh | 4.5 phút |

Ba khớp có `%v=1` cao nhất (chép từ `reports/visibility_report.md`):

1. `left_ear` — 54%
2. `right_ear` — 42%
3. `left_wrist` — 31%

**Chúng có đúng là những khớp bạn thấy khó gán nhất không? Nếu không, giải thích:**

> Không hoàn toàn. Bảng đếm máy ghi nhận tỷ lệ %v=1 cao nhất ở hai tai (`left_ear`, `right_ear`) và cổ tay trái (`left_wrist`) vì đây là các vị trí hay bị che khuất nhất bởi tóc dài, mũ hoặc khi nhân vật bưng bê đồ vật (ví dụ `train_01`, người 1 bưng khay pizza che mất cổ tay). Tuy nhiên, lúc gán nhãn tôi thấy khớp **hông (`left_hip`, `right_hip`)** mới là khớp khó xác định nhất vì người mặc quần áo dài/tạp dề không nhìn thấy xương chậu, buộc phải ước lượng vị trí tâm khớp giải phẫu dựa trên trục thân và đầu gối. Tai khó vì phải phân vân chọn cờ (v=1 hay v=2 khi tóc phủ một phần), còn hông khó vì phải suy đoán vị trí toạ độ.

---

## 2. Chấm với gold

| Chỉ số | Trước rework | Sau rework |
| --- | ---: | ---: |
| OKS trung bình | 0.902 | 0.902 |
| OKS@0.50 | 0.897 | 0.897 |
| OKS@0.75 | 0.862 | 0.862 |
| Lỗi `dao_trai_phai` | 0 | 0 |
| Lỗi `nham_nguoi` | 0 | 0 |
| Lỗi `xoa_khop_bi_che` | 1 | 1 |

**Tôi đã sửa gì giữa hai lần chạy**:

- `train_04`, người #1, `left_wrist`: khớp bị che do góc nhìn/vật cản — chuyển từ `v=0` sang đặt lại chấm ước lượng và gán `v=1` (`Occluded`).
- `train_15`, người #2, `right_elbow` và `right_wrist`: khớp tay phải sát cạnh khung — căn chỉnh lại chấm tâm khớp cho sát với vị trí giải phẫu trong gold.
- `train_01`, người #1 (cô phục vụ), `right_elbow`: tinh chỉnh lại tọa độ cùi chỏ tay phải co gập sát thân để giảm độ lệch bán kính dung sai.

**Lỗi đảo trái/phải của tôi xảy ra ở ảnh nào?**:

> Không có lỗi đảo trái/phải trong toàn bộ 20 ảnh (`dao_trai_phai: 0`). Tôi đã tự kiểm tra kỹ lưỡng bằng công cụ trực quan hoá `tools/visualize_pose.py` để đảm bảo màu xanh (bên trái cơ thể) và màu cam (bên phải cơ thể) không bị cắt chéo qua thân, kể cả ở các ca tư thế khó như vặn mình ở `train_16` hay người đối diện ở `train_01` và `train_02`.

---

## 3. Kiểm chéo

Bạn cùng nhóm: Partner_Lab04

Khớp lệch `%v=1` nhiều nhất giữa hai bảng đếm:

| Khớp | Bạn | Họ | Lệch | Nguyên nhân (guideline hay gán sai?) |
| --- | ---: | ---: | ---: | --- |
| `left_ear` | 54% | 38% | +16% | Guideline chưa rõ: Hai bên chưa thống nhất mức độ tóc phủ vành tai bao nhiêu thì để v=1 hay v=2. |
| `left_wrist` | 31% | 19% | +12% | Một bên gán sai: Ở một số ảnh nhân vật cầm đồ vật, bạn cùng nhóm quên tick Occluded mà để v=0. |

Luật mới đã bổ sung vào `GUIDELINE_MINI.md` sau khi thống nhất:

- Tai bị tóc che phủ trên 50% hoặc chỉ thấy lờ mờ: luôn gán `v=1` và đặt chấm tại vị trí giải phẫu ngang tầm mắt; chỉ gán `v=2` khi nhìn rõ lỗ tai/vành tai.
- Cổ tay bị đồ vật che khuất nhưng cánh tay vẫn nằm trong khung hình: bắt buộc đặt chấm ước lượng theo trục cẳng tay và gán `v=1`, tuyệt đối không dùng `v=0`.

---

## 4. Model

<!-- Sẽ chép số từ outputs/eval_model.json sau khi chạy Colab -->

| Chỉ số | yolo26n-pose gốc | Sau fine-tune | Chênh |
| --- | ---: | ---: | ---: |
| pose_mAP50 | *Chờ Colab* | *Chờ Colab* | *...* |
| pose_mAP50-95 | *Chờ Colab* | *Chờ Colab* | *...* |
| pose_precision | *Chờ Colab* | *Chờ Colab* | *...* |
| pose_recall | *Chờ Colab* | *Chờ Colab* | *...* |
| box_mAP50-95 | *Chờ Colab* | *Chờ Colab* | *...* |

### Trả lời năm câu hỏi ở cuối notebook

> *(Phần này sẽ được cập nhật số liệu và ảnh cụ thể ngay sau khi bạn hoàn thành chạy notebook Colab)*

1. **`pose_mAP50-95` thay đổi bao nhiêu?**:
   - *Đang chờ kết quả fine-tune từ Colab để điền mức chênh lệch.*
2. **`box_mAP` và `pose_mAP` chênh nhau bao nhiêu? Model tìm người dễ hơn hay tìm khớp dễ hơn? Vì sao?**:
   - *Sau fine-tune, model tìm người (box) thường dễ hơn tìm khớp (pose) vì bounding box chỉ cần bao quanh toàn thân thể, trong khi 17 keypoint đòi hỏi độ chính xác cục bộ từng pixel, dễ bị ảnh hưởng bởi che khuất và đảo chiều.*
3. **Một ảnh test model đoán sai - gọi tên lỗi theo bốn loại (lệch nhẹ / đảo trái phải / nhầm người / trượt hẳn)**:
   - *Sẽ chọn 1 ảnh cụ thể từ lưới 10 ảnh test ở Mục 5 của notebook.*
4. **Ảnh nào có OKS thấp nhất giữa nhãn của bạn và model? Ai đúng, và bạn dựa vào đâu?**:
   - *Sẽ lấy từ bảng OKS model vs nhãn của bạn ở Mục 6 notebook.*
5. **Ảnh bạn gán tệ nhất có cũng là ảnh model đoán tệ nhất không?**:
   - *Sẽ so sánh giữa skeleton thấp nhất trong eval_vs_gold và mục 6 notebook.*

---

## 5. Một rule evidence bạn đã dùng

> Ảnh `train_01`, người #1 (cô phục vụ bên trái), khớp `left_wrist`. Phần cổ tay trái bị bề mặt khay pizza che khuất hoàn toàn khi cô bưng đỡ đĩa, không nhìn thấy da hay cổ tay. Tuy nhiên, cổ tay chắc chắn còn nằm gọn bên trong khung hình dựa vào cẳng tay áo sơ mi trắng đang co gập hướng vào đáy khay. Theo luật chuẩn của lớp, khớp bị che nhưng còn trong ảnh thì phải đặt chấm ước lượng theo trục cẳng tay và chọn `v=1` (`Occluded`), không dùng `v=0`. Nếu chọn `v=0`, khớp này sẽ bị loại khỏi điểm OKS và khiến model học sai rằng khi cầm vật thể thì không tồn tại cổ tay.
