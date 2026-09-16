# Visibility report

- Thư mục nhãn: `dataset\labels\train`
- 20 ảnh, 26 skeleton, trung bình 15.35 khớp có v > 0 mỗi người
- Tổng: v=2 299 | v=1 100 | v=0 43

| # | Khớp | v=2 | v=1 | v=0 | %v=1 |
| ---: | --- | ---: | ---: | ---: | ---: |
| 0 | nose | 20 | 5 | 1 | 19% |
| 1 | left_eye | 20 | 6 | 0 | 23% |
| 2 | right_eye | 20 | 6 | 0 | 23% |
| 3 | left_ear | 12 | 14 | 0 | 54% |
| 4 | right_ear | 15 | 11 | 0 | 42% |
| 5 | left_shoulder | 24 | 2 | 0 | 8% |
| 6 | right_shoulder | 25 | 1 | 0 | 4% |
| 7 | left_elbow | 19 | 5 | 2 | 19% |
| 8 | right_elbow | 18 | 5 | 3 | 19% |
| 9 | left_wrist | 15 | 8 | 3 | 31% |
| 10 | right_wrist | 18 | 4 | 4 | 15% |
| 11 | left_hip | 20 | 6 | 0 | 23% |
| 12 | right_hip | 20 | 6 | 0 | 23% |
| 13 | left_knee | 16 | 4 | 6 | 15% |
| 14 | right_knee | 14 | 5 | 7 | 19% |
| 15 | left_ankle | 13 | 5 | 8 | 19% |
| 16 | right_ankle | 10 | 7 | 9 | 27% |

## Đọc bảng này thế nào

1. Khớp nào có **%v=1 cao**: khớp hay bị che. Cổ tay và hông thường là hai vị trí cần xem lại guideline trước khi kết luận.
2. Khớp nào có **v=0 cao bất thường**: mọi người đang dùng Outside ở chỗ đáng lẽ là Occluded. Đó là lỗi số 3 của slide 46, và nó xoá thẳng khớp đó khỏi bảng điểm OKS.
3. Khi so hai người: **lệch lớn = bất đồng về guideline**, không phải về bức ảnh. Sửa guideline trước, sửa nhãn sau.
