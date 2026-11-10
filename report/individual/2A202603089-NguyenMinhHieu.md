# Báo cáo cá nhân — Ngày 2 Object Detection

Đổi tên file theo `MSSV_HoTen.md`, ví dụ `20241234_NguyenVanAn.md`. Điền kết quả thật, xóa dòng hướng dẫn khi nộp. Xem [quy định nộp](../../SUBMISSION.md).

## 1. Thông tin học viên

- Họ tên: Nguyễn Minh Hiếu
- MSSV: 2A202603089
- Email: hieunguyenminh0412@gmail.com
- Lớp: 2A-LAB-E402
- Link repo bài làm: https://github.com/FrostBite1707/K5-L2A-DAY02-NguyenMinhHieu-2A202603089-ObjectDetection
- Link Colab đã chạy: https://colab.research.google.com/drive/1imY-7oAKridD3c7DKcNWRkXpdrW4b4yE#scrollTo=3e0508d8

## 2. Schema và mini guideline

Dẫn tới [GUIDELINE_MINI.md](../../GUIDELINE_MINI.md). Tóm tắt ba ca mơ hồ đã quyết định, cách áp dụng schema và phần cần giảng viên làm rõ.

## 3. Annotation và self-QC

- Số file nhãn train: Có 50 file nhãn train
- Kết quả checker và lỗi đã xử lý: `python3 tools/check_yolo_labels.py` — đạt cấu trúc 50/50 file; checker không phát hiện lỗi định dạng. Checker chỉ kiểm tra cấu trúc, không xác nhận nội dung box/class.
- Danh sách ít nhất 10 ảnh đã rà bằng mắt: drive_001.jpg, drive_002.jpg, drive_003.jpg, drive_004.jpg, drive_005.jpg, drive_006.jpg, drive_007.jpg, drive_008.jpg, drive_009.jpg, drive_010jpg, drive_011.jpg.
- Lỗi bỏ sót/box/class phát hiện khi rà và cách sửa:
* Bus 46 có box bị quá hẹp làm cắt mất 1 phần gương -> mở rộng box bao gồm cả gương.
* Car 236 bị gán nhãn nhầm từ xe bus thành xe con -> sửa lại thành bus 236.
- Commit khóa nhãn ban đầu: Commit nhãn hiện có là 7a5f97e (Add annotation data).

## 4. Gold QC và rework

| Chỉ số | Trước sửa | Sau sửa nếu có |
|---|---:|---:|
| TP | 474 | 474 |
| FP | 524 | 524 |
| FN | 179 | 179 |
| Precision | 0.4749 | 0.4749 |
| Recall | 0.7259 | 0.7259 |
| F1 | 0.5742 | 0.5742 |
| mean_iou_of_matches | 0.8462 | 0.8461 |
| passed | false | false |

Dẫn tới [`outputs/annotation_evaluation.json`](../../outputs/annotation_evaluation.json). Rework thử nghiệm: đổi class ID theo thứ tự người học báo đã dùng (`car, van, truck, bus`) sang thứ tự trong `data.yaml` (`car, truck, bus, van`), giữ nguyên tọa độ box. Checker đạt 50/50; evaluator sau đổi vẫn chưa đạt: F1 0.5742 (<0.80), mean IoU 0.8461 (>=0.70), TP/FP/FN không đổi. Phép đổi class ID này chưa khắc phục được chênh lệch; cần tiếp tục rà từng đối tượng/box với ảnh và gold trước khi quyết định sửa thêm.
## 5. Kết quả YOLO

- Trạng thái: Chưa chạy train/đánh giá YOLO. Sau rework thử nghiệm đổi class ID, Gold QC vẫn chưa đạt (`F1=0.5742`, yêu cầu `>=0.80`), nên chưa qua điều kiện train trong notebook.
- Runtime, GPU/CPU, phiên bản Ultralytics và Torch: Chưa có phiên chạy để ghi nhận.
- Tham số trong notebook (chưa xác nhận đã chạy): `yolo26n.pt`, 50 epochs, `imgsz=640`, `batch=-1`, seed `20260909`; không có thay đổi tham số được ghi nhận.
- Đường dẫn best checkpoint trong phiên chạy: Chưa có — notebook chưa tạo checkpoint trong workspace.

| Chỉ số trên 10 ảnh eval | Giá trị |
|---|---:|
| Precision | Chưa có kết quả |
| Recall | Chưa có kết quả |
| mAP50 | Chưa có kết quả |
| mAP50–95 | Chưa có kết quả |

Chưa có `day2_colab_outputs.zip` hoặc kết quả tương ứng trong workspace; `assets/eval_predictions/` và `assets/yolo26_runs/` chưa được tạo (hiện chỉ có `.gitkeep`). Chưa thể nêu ảnh/vật thể dựa trên prediction thực tế.

Trả lời bốn câu hỏi cuối notebook (các câu cần output thực tế được đánh dấu):

1. Chưa xác định được lớp recall thấp nhất hoặc ảnh minh họa vì chưa có kết quả validation theo lớp và ảnh prediction.
2. Kiểm tra box có lệch khỏi xe, quá rộng/hẹp hoặc chưa ôm sát biên vật thể không. Box có thể đạt IoU ở ngưỡng 0.50 nhưng trượt ở các ngưỡng IoU cao hơn, kéo mAP50–95 xuống.
3. Chưa xác định lỗi class nào liên quan guideline vì chưa có confusion matrix/ảnh prediction của phiên train để đối chiếu với mini guideline.
4. Không dùng 10 ảnh eval để train nhằm giữ chúng ngoài dữ liệu cập nhật trọng số, tránh rò rỉ dữ liệu. Trong notebook này, `val` dùng 10 ảnh eval để theo dõi validation và chọn `best.pt`; vì vậy đây không phải test set độc lập.

## 6. Khó khăn và cách xử lý

| Bước/thời điểm | Lỗi quan sát | Cách đã thử | Kết quả/còn thiếu |
|---|---|---|---|
| | | | |

## 7. Tự đánh giá theo rubric

Đối chiếu [RUBRIC.md](../../RUBRIC.md): phần nào đã có kết quả, phần nào còn thiếu hoặc chưa kiểm được. Không tự nhận hoàn thành bước chưa chạy.

## 8. Công cụ AI và tài liệu tham khảo

Ghi công cụ đã dùng, dùng ở bước nào và phần đã tự kiểm tra; không dùng thì ghi rõ. Liệt kê nguồn ngoài repo được sử dụng. Không ghi mật khẩu/token hoặc đính kèm gold train.
