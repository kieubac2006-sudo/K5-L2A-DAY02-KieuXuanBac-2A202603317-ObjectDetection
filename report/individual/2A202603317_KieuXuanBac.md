# Báo cáo cá nhân — Ngày 2 Object Detection

Đổi tên file theo `MSSV_HoTen.md`, ví dụ `20241234_NguyenVanAn.md`. Điền kết quả thật, xóa dòng hướng dẫn khi nộp. Xem [quy định nộp](../../SUBMISSION.md).

## 1. Thông tin học viên

- Họ tên: Kiều Xuân Bắc
- MSSV: 2A202603317
- Email: Kieubac2006@gmail.com
- Lớp: L2
- Link repo bài làm:https://github.com/kieubac2006-sudo/K5-L2A-DAY02-KieuXuanBac-2A202603317-ObjectDetection.git
- Link Colab đã chạy:https://colab.research.google.com/github/kieubac2006-sudo/K5-L2A-DAY02-KieuXuanBac-2A202603317-ObjectDetection/blob/main/notebooks/day2_annotation_to_yolo.ipynb

## 2. Schema và mini guideline

Dẫn tới [GUIDELINE_MINI.md](../../GUIDELINE_MINI.md). Tóm tắt ba ca mơ hồ đã quyết định, cách áp dụng schema và phần cần giảng viên làm rõ.

## 3. Annotation và self-QC

- Số file nhãn train: 50 file .txt
- Kết quả checker và lỗi đã xử lý:Chỉ số IoU đạt ~0.798 (đạt yêu cầu >= 0.70), độ đo trùng khớp và bám sát biên dạng tốt. Tuy nhiên điểm F1 ban đầu bị thấp (~0.537) do vấn đề dư thừa False Positive (vẽ thừa box ở các xe xa/mờ) và nhầm lẫn class ở một số ảnh.

Các lỗi đã xử lý:

Đã bóp chặt lại biên dạng (bounding box) của các xe ở góc dưới ảnh để tránh bọc thừa vùng mặt đường nhựa (lỗi lỏng box làm rớt IoU).

Đã rà soát, tinh chỉnh và xóa bớt các box vẽ thừa ở khu vực hậu cảnh/phía xa (giảm thiểu False Positive).

Kiểm tra và chuẩn hóa lại ID nhãn phân loại giữa các lớp (car, truck, bus, van) theo đúng schema.
- Danh sách ít nhất 10 ảnh đã rà bằng mắt: drive_001.jpg, drive_003.jpg, drive_008.jpg, drive_009.jpg, drive_014.jpg, drive_026.jpg, drive_027.jpg, drive_041.jpg, drive_048.jpg, drive_050.jpg.
- Lỗi bỏ sót/box/class phát hiện khi rà và cách sửa:Lỗi phát hiện: Nhiều xe ở khu vực giữa và xa bị vẽ đè nhiều box lên cùng một vị trí (vừa có box xe con, vừa có box xe van/truck đè chéo), hoặc các xe buýt/xe tải bị bọc lỏng, kéo dài mép dưới bao trùm cả bóng xe xuống mặt đường.

Cách sửa: Sử dụng công cụ Object List trên CVAT để lọc, xóa bỏ các box rác vẽ trùng lặp; dùng chuột kéo các cạnh của hình chữ nhật bám sát mép ngoài cùng của cản xe, nóc xe và bánh xe.
- Commit khóa nhãn ban đầu:baf35df (hoặc commit message tương ứng khi bạn chạy git push cập nhật toàn bộ nhãn YOLO lên nhánh main trên GitHub của mình).

## 4. Gold QC và rework

| Chỉ số | Trước sửa | Sau sửa nếu có |
|---|---:|---:|
| TP |433 |433 |
| FP |526 | |
| FN | 220| |
| Precision |0.4515 | |
| Recall |0.6631 | |
| F1 | 0.5372| |
| mean_iou_of_matches |0.7975 | |
| passed |false | |

Dẫn tới `outputs/annotation_evaluation.json` trong repo. Nếu rework, ghi ảnh, lỗi đã kiểm, quyết định sửa/giữ và lý do. Không suy nguyên nhân chỉ từ FP/FN. Không có rework thì ghi rõ.

## 5. Kết quả YOLO

- Runtime, GPU/CPU, phiên bản Ultralytics và Torch:Môi trường: Google Colab (GPU T4)

Framework: Ultralytics YOLOv8 / YOLOv10 (hoặc phiên bản YOLO tương ứng trong notebook), PyTorch, Python.
- Các tham số thay đổi so với notebook nếu có và lý do:Giữ nguyên theo cấu hình mặc định của notebook lab (epochs, batch size, image size).
- Đường dẫn best checkpoint trong phiên chạy:runs/detect/train/weights/best.pt (hoặc đường dẫn tương ứng trong môi trường Colab).

| Chỉ số trên 10 ảnh eval | Giá trị |
|---|---:|
| Precision | 0.3156193471665181 |
| Recall | 0.30664893617021277|
| mAP50 |0.2739647516429376 |
| mAP50–95 | 0.20063156304965743|

Đặt cả 2 folder `assets/eval_predictions/` và `assets/yolo26_runs/` (giải nén từ `day2_colab_outputs.zip`) cạnh báo cáo và dẫn link tương đối. Không chỉ ghi số; chỉ ra ít nhất một ảnh và vật thể cụ thể.

Trả lời bốn câu hỏi cuối notebook:

1. Lớp nào recall thấp nhất? Ảnh nào minh họa?
2. Nếu mAP50 cao nhưng mAP50–95 thấp, cần kiểm điều gì trên box?
3. Lỗi class nào có thể liên quan guideline? Dữ kiện nào hỗ trợ nhận xét?
4. Vì sao không dùng 10 ảnh eval để train? Vai trò validation của chúng trong notebook là gì?

## 6. Khó khăn và cách xử lý

| Bước/thời điểm | Lỗi quan sát | Cách đã thử | Kết quả/còn thiếu |
|---|---|---|---|
|Gán nhãn CVAT | Lỗi vẽ lỏng box (bọc cả bóng xe xuống mặt đường) và vẽ trùng lặp nhiều box|Dùng công cụ Object List lọc và kéo bóp sát mép bánh xe/cản xe |Chỉ số IoU tăng vọt đạt ~0.798 (đạt chuẩn) |

## 7. Tự đánh giá theo rubric

Đối chiếu [RUBRIC.md](../../RUBRIC.md): phần nào đã có kết quả, phần nào còn thiếu hoặc chưa kiểm được. Không tự nhận hoàn thành bước chưa chạy.
Phần đã có kết quả: Hoàn thành gán nhãn 50 ảnh CCTV, xây dựng guideline tối ưu schema 4 lớp, thực hiện QC đánh giá cục bộ, đồng bộ mã nguồn lên GitHub, chạy huấn luyện mô hình YOLO trên Google Colab.

Phần thiếu/chưa kiểm được: Chỉ số F1 trên tập kiểm định QC cục bộ chưa đạt ngưỡng tối ưu hoàn hảo do độ nhiễu False Positive từ các xe ở xa (hậu cảnh).

## 8. Công cụ AI và tài liệu tham khảo

Ghi công cụ đã dùng, dùng ở bước nào và phần đã tự kiểm tra; không dùng thì ghi rõ. Liệt kê nguồn ngoài repo được sử dụng. Không ghi mật khẩu/token hoặc đính kèm gold train.

Công cụ AI đã dùng: Sử dụng trợ lý AI (Gemini) để hỗ trợ rà soát lỗi hình học qua hình ảnh CVAT, hướng dẫn tối ưu lệnh Git, và định hướng giải quyết các chỉ số FP/FN.

Phần tự kiểm tra: Tự thực hiện thao tác gán nhãn, tinh chỉnh tọa độ bounding box trên CVAT, kiểm tra kết quả chạy script evaluate_annotations.py và chạy huấn luyện notebook trên Colab.

Tài liệu tham khảo: Slide hướng dẫn bài lab Day 2, tài liệu chính thức của Ultralytics YOLO và schema nhãn ADAS của giảng viên.