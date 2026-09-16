# Mini guideline - nhóm: Lab04-Group  |  người gán: Học viên  |  ngày: 2026-09-16

> Điền file này **trong lúc** gán nhãn, không phải sau khi xong. Mỗi lần bạn dừng lại
> hơn 10 giây để phân vân, đó là một dòng phải ghi vào đây.

## 1. Luật bắt buộc (đã thống nhất cả lớp - không sửa)

- Bộ 17 điểm COCO, đúng tên, đúng thứ tự. Lấy từ file `.SVG` chung.
- Mọi người trong ảnh đều có **đủ 17 điểm**. Điểm không dùng được thì gắn cờ, không xoá.
- Trái/phải tính theo **cơ thể người**, không theo bức ảnh.
- Bị che, còn trong khung -> `v = 1`, **vẫn đặt chấm** ở vị trí ước lượng.
- Ra ngoài mép ảnh -> `v = 0`, **không** đặt chấm.
- Không dùng `Hidden` (`h`) - nó không được lưu vào file.

## 2. Luật của nhóm bạn (phải điền)

| Tình huống | Luật nhóm bạn chọn | Vì sao |
| --- | --- | --- |
| Hông của người mặc quần áo dài | Đặt tại khớp háng giải phẫu (ngang cạp quần/đáy xương chậu), gán `v=1` | Quần áo dài che mất xương hông nhưng vị trí giải phẫu vẫn suy ra được từ trục thân và đùi |
| Tai bị tóc hoặc mũ bảo hiểm che một phần | Kéo điểm vào vị trí ước lượng khớp tai (ngang tầm mắt/sống mũi) và gán `v=1` | Vẫn xác định được tương đối vị trí tai dựa trên góc nghiêng khuôn mặt và cấu trúc đầu |
| Người bị cắt ở mép ảnh (chỉ thấy từ hông trở lên) | Các khớp nằm ngoài mép cắt (gối, cổ chân) chọn và bấm `o` (`v=0`) | Khớp đã ra ngoài hoàn toàn khung ảnh, không đoán mò toạ độ |
| Cổ tay nằm sau tay lái / sau thân mình / bị đĩa che | Kéo điểm vào vị trí ước lượng theo hướng cẳng tay và bấm `q` (`v=1`) | Khớp vẫn nằm trong ảnh và có thể nội suy chính xác từ hướng đi của cẳng tay |
| Hai người chồng lên nhau | Gán từng người hoàn chỉnh riêng biệt, khớp người sau bị người trước che thì gán `v=1` | Đảm bảo không nhầm lẫn đường nối xương giữa 2 cơ thể và duy trì đủ 17 điểm cho mỗi người |
| Người nhỏ đến mức nào thì không gán nữa | Gán tất cả mọi người có thể phân biệt được ít nhất phần đầu/thân trên | Bộ dữ liệu Lab 4 đã được chuẩn hoá để gán toàn bộ nhân vật nhìn thấy trong 20 ảnh |

## 3. Ba ca mơ hồ đã gặp (bắt buộc, ghi ít nhất 3)

### Ca 1 - ảnh `train_01`, người thứ `1` (cô phục vụ), khớp `left_wrist`

- Mơ hồ ở chỗ nào: Cổ tay trái của cô gái nằm hoàn toàn dưới đáy khay pizza đang bưng, không nhìn thấy bề mặt cổ tay.
- Bạn quyết thế nào: Kéo điểm vào vị trí ước lượng nơi cổ tay đỡ khay pizza (theo hướng cánh tay co gập) và gán `v=1` (`Occluded`).
- Vì sao: Khớp vẫn ở trong khung ảnh, cánh tay áo và tư thế bưng khay cho bằng chứng rõ ràng về vị trí cổ tay.
- Nếu người khác quyết ngược lại (gán `v=0`): Model sẽ học sai rằng khi có vật che thì khớp biến mất, làm giảm khả năng nhận diện pose khi bưng bê đồ vật.

### Ca 2 - ảnh `train_16`, người thứ `2` (áo đỏ số 23), khớp `left_shoulder` / `right_shoulder`

- Mơ hồ ở chỗ nào: Người này quay lưng về phía camera nhưng ngoái đầu sang phải để đón đĩa bay, gây cảm giác ngược chiều giữa mắt và vai.
- Bạn quyết thế nào: Giữ nguyên quy tắc giải phẫu cơ thể: vai trái (`left_shoulder`) ở bên trái màn hình (phía sau lưng), vai phải (`right_shoulder`) ở bên phải màn hình (vươn cao), gán `v=2`.
- Vì sao: Trái/phải luôn tính theo cơ thể người. Dù đầu ngoái nhưng vai và cột sống không đổi vị trí giải phẫu.
- Nếu người khác quyết ngược lại (đảo trái/phải theo hướng mặt): Model sẽ bị lỗi SWAP nghiêm trọng khi học các tư thế vặn mình thể thao.

### Ca 3 - ảnh `train_01`, người thứ `1` và `2`, khớp `left_ankle` / `right_ankle`

- Mơ hồ ở chỗ nào: Cả hai người đều bị cắt ngang qua đùi/đầu gối ở mép dưới bức ảnh.
- Bạn quyết thế nào: Chọn cả 2 điểm `left_ankle`, `right_ankle` và bấm phím `o` (`v=0` Outside).
- Vì sao: Cổ chân nằm hoàn toàn ngoài khung hình, không có cơ sở thị giác hay pixel nào trong ảnh.
- Nếu người khác quyết ngược lại (cố kéo điểm ra sát mép ảnh với `v=1` hoặc `v=2`): Model sẽ học tọa độ ảo ở biên ảnh và sinh ra dự đoán trôi (float).

## 4. Sau khi so visibility report với bạn cùng nhóm

- Khớp lệch `%v=1` nhiều nhất: `left_ear` (bạn `54%` / họ `38%`)
- Nguyên nhân là **guideline chưa rõ**: Mức độ tóc phủ tai bao nhiêu thì tính là `v=1` hay `v=2` chưa thống nhất.
- Luật mới bổ sung vào mục 2 sau khi thống nhất: Chỉ gán `v=2` khi nhìn thấy rõ vành tai hoặc lỗ tai; nếu tóc phủ kín hoàn toàn hoặc chỉ thấy lờ mờ dưới lọn tóc thì luôn gán `v=1`.
