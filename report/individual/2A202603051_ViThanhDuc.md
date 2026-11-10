# Báo cáo cá nhân — Ngày 2 Object Detection

Đổi tên file theo `MSSV_HoTen.md`, ví dụ `20241234_NguyenVanAn.md`. Điền kết quả thật, xóa dòng hướng dẫn khi nộp. Xem [quy định nộp](../../SUBMISSION.md).

## 1. Thông tin học viên

- Họ tên:
- MSSV:
- Email:
- Lớp: 
- Link repo bài làm: https://github.com/viduc2811/K5-L2-DAY02-ViThanhDuc-2A202603051-ObjectDetection
- Link Colab đã chạy: https://colab.research.google.com/drive/1iiXQdDjuvo1pt7zL8ZRG4pR6u1PVVgY4#scrollTo=3e0508d8

## 2. Schema và mini guideline

Dẫn tới [GUIDELINE_MINI.md](../../GUIDELINE_MINI.md). Tóm tắt ba ca mơ hồ đã quyết định, cách áp dụng schema và phần cần giảng viên làm rõ.
 Một phương tiện – một bounding box: Không tách một phương tiện thành nhiều box chỉ vì phương tiện có nhiều khoang hoặc cấu tạo đặc biệt.

 Xử lý vật thể bị che khuất: Nếu vẫn nhận dạng được phương tiện, tiến hành gán nhãn theo phần vật thể nhìn thấy được, không tạo thêm box cho phần bị che khuất.

 Phân biệt các lớp phương tiện: Dựa vào đặc điểm hình dáng và cấu tạo tổng thể của phương tiện để lựa chọn class phù hợp, tránh chỉ dựa vào kích thước hoặc màu sắc.

 Tuân thủ schema: Sử dụng đúng tên lớp được định nghĩa, không tự ý tạo class mới hoặc thay đổi quy tắc gán nhãn.

## 3. Annotation và self-QC

- Số file nhãn train: 50
- Kết quả checker và lỗi đã xử lý: Pass
 Các lỗi đã xử lý:
  Một số xe con bị gán thành truck, xe tải bị gán thành bus và xe buýt bị gán thành truck. Đã đối chiếu hình dạng phương tiện với guideline và sửa lại class.
  Phát hiện một số xe có thể nhận rõ đặc điểm chưa được gán nhãn. Đã bổ sung bounding box.
  Phát hiện box gán nhầm cho các thùng hàng mà xe truck chở, mái che,... Đã xóa.

- Danh sách ít nhất 10 ảnh đã rà bằng mắt: drive_001, drive_002, drive_003, drive_004, drive_005, drive_006, drive_007, drive_008, drive_009, drive_010, 
- Lỗi bỏ sót/box/class phát hiện khi rà và cách sửa:
 Một số phương tiện xuất hiện trong ảnh nhưng chưa được gán nhãn. Cách sửa: Bổ sung bounding box cho các đối tượng bị bỏ sót.
 Các box chưa bao sát đối tượng, kích thước hoặc vị trí chưa chính xác. Cách sửa: Điều chỉnh lại tọa độ và kích thước box.
 Box được gán nhầm cho vật thể không thuộc đối tượng cần nhận diện. Cách sửa: Xóa các box không hợp lệ.
 Phương tiện bị nhầm car thành van, van thành truck. Cách sửa: Kiểm tra đặc điểm phương tiện và đổi sang class phù hợp theo guideline.

- Commit khóa nhãn ban đầu: c736f9e 'commit'

## 4. Gold QC và rework

| Chỉ số | Trước sửa | Sau sửa nếu có |
|---|---:|---:|
| TP |434 | |
| FP |454 | |
| FN |219 | |
| Precision |0.4887 | |
| Recall |0.6646 | |
| F1 |0.5633 | |
| mean_iou_of_matches |0.8529 | |
| passed |false | |

Kết quả là `false` so với ngưỡng gốc F1 ≥ 0,80, nhưng lab coach đã xác nhận F1 > 0,5 là chấp nhận được cho bài này.

Dẫn tới `outputs/annotation_evaluation.json` trong repo. Nếu rework, ghi ảnh, lỗi đã kiểm, quyết định sửa/giữ và lý do. Không suy nguyên nhân chỉ từ FP/FN. Không có rework thì ghi rõ.

## 5. Kết quả YOLO

- Runtime, GPU/CPU, phiên bản Ultralytics và Torch: Ultralytics 8.4.208  Python-3.13.16 torch-2.14.1+cu130 CUDA:0 (Tesla T4, 14913MiB)
- Các tham số thay đổi so với notebook nếu có và lý do: Không đổi
- Đường dẫn best checkpoint trong phiên chạy:  /content/Day2-Lab/outputs/yolo26_runs/vehicle_annotation/weights/best.pt

| Chỉ số trên 10 ảnh eval | Giá trị |
|---|---:|
| Precision |0.316 |
| Recall |0.307 |
| mAP50 |0.274 |
| mAP50–95 |0.201 |

Đặt cả 2 folder `assets/eval_predictions/` và `assets/yolo26_runs/` (giải nén từ `day2_colab_outputs.zip`) cạnh báo cáo và dẫn link tương đối. Không chỉ ghi số; chỉ ra ít nhất một ảnh và vật thể cụ thể.

Trả lời bốn câu hỏi cuối notebook:

1. Lớp nào recall thấp nhất? Ảnh nào minh họa?
 Lớp có recall thấp nhất: Bus có recall = 0, ảnh minh hoạ: drive_052, drive_053, drive_056, drive_058, drive_060
2. Nếu mAP50 cao nhưng mAP50–95 thấp, cần kiểm điều gì trên box?
 mAP50 = 0.274 và mAP50–95 = 0.201 cho thấy mAP50–95 thấp hơn mAP50. Điều này cho thấy các bounding box chưa đủ chính xác khi đánh giá ở những ngưỡng IoU cao hơn. Cần kiểm tra và chỉnh lại tọa độ, chiều rộng, chiều cao của box để box khớp sát với đối tượng hơn.
3. Lỗi class nào có thể liên quan guideline? Dữ kiện nào hỗ trợ nhận xét?
 Một số xe Van bị dự đoán nhầm thành Car vì xe van nhỏ khó phân biệt guideline mô tả van là thân hộp, cao hơn car, nhưng với xe nhỏ ở xa tiêu chí này khó áp dụng và truck dễ nhầm với bus/car ở xe thùng
  Dữ liệu hỗ trợ nhận xét: drive_056 có xe nhầm van thành car 
4. Vì sao không dùng 10 ảnh eval để train? Vai trò validation của chúng trong notebook là gì?
 Không sử dụng 10 ảnh eval để train vì tập eval dùng để đánh giá khả năng tổng quát hóa (Generalization) của mô hình trên dữ liệu chưa được sử dụng để huấn luyện. Nếu đưa các ảnh này vào quá trình train, kết quả mAP có thể bị thổi phồng và không còn phản ánh khách quan chất lượng mô hình.
 Validation Set giúp xác định phiên bản mô hình có hiệu suất tốt nhất trong quá trình huấn luyện.

## 6. Khó khăn và cách xử lý

| Bước/thời điểm | Lỗi quan sát | Cách đã thử | Kết quả/còn thiếu |
|---|---|---|---|
|Chấm gold label |Các box xe nhỏ không có trong gold |Bỏ các xe nhỏ ở xa, mờ |Không áp dụng vì mất xe thật, chờ giảng viên làm rõ quy tắc xe car trong gold |
|Train mô hình |Colab lỗi, không thể kết nối drive |Kiểm tra kết nối, chạy lại |Thành công|
|Chấm gold label |Các xe bị gán nhầm label|Đọc lại guideline, phân biệt rõ các xe |Gán đúng label|

## 7. Tự đánh giá theo rubric

Đối chiếu [RUBRIC.md](../../RUBRIC.md): phần nào đã có kết quả, phần nào còn thiếu hoặc chưa kiểm được. Không tự nhận hoàn thành bước chưa chạy.
Hoàn thành và định dạng YOLO Đạt đủ 50/50
Đầy đủ vật thể: còn nhiều xe nhỏ ở xa mà em gán còn gold không gán
Độ bám sát và hình học box: Độ bám sát tốt "mean_iou_of_matches": 0.8529
Phân lớp nhất quán: Còn nhầm lẫn car với van, truck với van
Mini guideline và quyết định mơ hồ: Đã định nghĩa lớp, quy tắc
Gold-set annotation QC: F1: 0.5633, mean IoU: 0.8529. F1 và mean IoU đã đạt do Lab coach báo chỉ yêu cầu F1>=0.5
Pipeline YOLO và phân tích: Hoàn thành

## 8. Công cụ AI và tài liệu tham khảo

Ghi công cụ đã dùng, dùng ở bước nào và phần đã tự kiểm tra; không dùng thì ghi rõ. Liệt kê nguồn ngoài repo được sử dụng. Không ghi mật khẩu/token hoặc đính kèm gold train.
 Không dùng AI cho dán nhãn
 Công cụ khác: CVAT chạy local (Docker) để gán nhãn và xuất YOLO 1.1; thư viện Ultralytics; Google Colab; GitHub.