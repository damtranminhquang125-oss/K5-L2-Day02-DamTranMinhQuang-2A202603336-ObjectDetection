# Báo cáo cá nhân — Ngày 2 Object Detection

Đổi tên file theo `MSSV_HoTen.md`, ví dụ `20241234_NguyenVanAn.md`. Điền kết quả thật, xóa dòng hướng dẫn khi nộp. Xem [quy định nộp](../../SUBMISSION.md).

## 1. Thông tin học viên

- Họ tên: Đàm Trần Minh Quang
- MSSV: 2A202603336
- Email: 26ai.quangdtm@vinuni.edu.vn
- Lớp: L2B
- Link repo bài làm: https://github.com/damtranminhquang125-oss/K5-L2-Day02-DamTranMinhQuang-2A202603336-ObjectDetection
- Link Colab đã chạy: https://colab.research.google.com/drive/1yi7Hk5pmBDu4tkREQIzAGlriJeeSc3dV?usp=sharing

## 2. Schema và mini guideline

Dẫn tới [GUIDELINE_MINI.md](../../GUIDELINE_MINI.md). Tóm tắt ba ca mơ hồ đã quyết định, cách áp dụng schema và phần cần giảng viên làm rõ.

## 3. Annotation và self-QC

- Số file nhãn train: 50
- Kết quả checker và lỗi đã xử lý: Đã đạt checker, đã sửa nhãn theo lỗi FN/FP và box lỏng
- Danh sách ít nhất 10 ảnh đã rà bằng mắt: drive_001, drive_002, drive_003, drive_004, drive_005, drive_006, drive_007, drive_008, drive_009, drive_010
- Lỗi bỏ sót/box/class phát hiện khi rà và cách sửa: Label tiếp box/class không quá nhỏ hoặc bị che khuất ít.
- Commit khóa nhãn ban đầu: 

## 4. Gold QC và rework

| Chỉ số | Trước sửa | Sau sửa nếu có |
|---|---:|---:|
| TP | 261 | 517 |
| FP | 129 | 17 |
| FN | 392 | 136 |
| Precision | 0.6692 | 0.9682 |
| Recall | 0.3997 | 0.7917 |
| F1 | 0.5005 | 0.8711 |
| mean_iou_of_matches | 0.8713 | 0.9134 |
| passed | Chưa Pass | Pass |

Dẫn tới `outputs/annotation_evaluation.json` trong repo. Nếu rework, ghi ảnh, lỗi đã kiểm, quyết định sửa/giữ và lý do. Không suy nguyên nhân chỉ từ FP/FN. Không có rework thì ghi rõ.

## 5. Kết quả YOLO

- Runtime, GPU/CPU, phiên bản Ultralytics và Torch:
- Các tham số thay đổi so với notebook nếu có và lý do:
- Đường dẫn best checkpoint trong phiên chạy:

| Chỉ số trên 10 ảnh eval | Giá trị |
|---|---:|
| Precision | 0.530610792892006 |
| Recall | 0.5756205673758865 |
| mAP50 | 0.49541045691872393 |
| mAP50–95 | 0.3343740566105904 |

Đặt cả 2 folder `assets/eval_predictions/` và `assets/yolo26_runs/` (giải nén từ `day2_colab_outputs.zip`) cạnh báo cáo và dẫn link tương đối. Không chỉ ghi số; chỉ ra ít nhất một ảnh và vật thể cụ thể.

Trả lời bốn câu hỏi cuối notebook:

1. Lớp nào recall thấp nhất? Ảnh nào minh họa?
Dựa trên bảng kết quả đánh giá (Validation/Eval metric), lớp truck (Recall = 0.400) là lớp có recall thấp nhất. 
Ảnh eval minh họa: Ở ảnh drive_051.jpg, mô hình chỉ nhận diện được 1 truck, tuy nhiên do recall thấp, mô hình rất dễ bỏ sót các xe cỡ lớn này ở phía xa hoặc bị che khuất một phần.
2. Nếu mAP50 cao nhưng mAP50–95 thấp, cần kiểm điều gì trên box?
Nếu $mAP_{50}$ cao (mô hình nhận diện và phân loại đúng vật thể ở mức IoU tối thiểu 0.5) nhưng $mAP_{50-95}$ thấp, điều này có nghĩa là độ chính xác về tọa độ của bounding box chưa tốt. Các bounding box dự đoán có thể bị lệch, quá rộng, hoặc quá hẹp so với nhãn chuẩn (ground truth), dẫn đến việc khi siết chặt điều kiện trùng khớp IoU (từ 0.5 lên đến 0.95), số lượng box đạt chuẩn giảm mạnh.
3. Lỗi class nào có thể liên quan guideline? Dữ kiện nào hỗ trợ nhận xét?
- Nhầm lẫn giữa van và car/truck: Các dòng xe bán tải, xe 7 chỗ cỡ lớn (SUV) dễ bị gán nhãn sai lệch giữa car và van hoặc van và truck nếu người gán nhãn không bám sát định nghĩa kích thước/mục đích sử dụng của guideline.
- Nhầm lẫn giữa truck và van: Các xe tải nhỏ dạng cabin kín (box truck nhỏ) thường bị gán nhầm thành van, hoặc ngược lại.
- Bao phủ không sát: Không bao gồm cả gương chiếu hậu hoặc bao quá rộng cả phần bóng của xe dưới đường.
4. Vì sao không dùng 10 ảnh eval để train? Vai trò validation của chúng trong notebook là gì?
Tránh hiện tượng Data Leakage: Nếu đưa 10 ảnh eval vào tập train, mô hình sẽ "học vẹt" các đặc trưng của những ảnh này. Chúng ta cần giữ lại một tập dữ liệu hoàn toàn độc lập mà mô hình chưa từng nhìn thấy trong quá trình cập nhật trọng số để đo lường khả năng tổng quát hóa thực tế của mô hình trên dữ liệu mới.

## 6. Khó khăn và cách xử lý

| Bước/thời điểm | Lỗi quan sát | Cách đã thử | Kết quả/còn thiếu |
|Label|Các xe dừng đèn đó đứng che khuất lẫn nhau|Cố label toàn bộ các phương tiện|Vài box không sát, bị nhầm lớp|
| | | | |

## 7. Tự đánh giá theo rubric

Đối chiếu [RUBRIC.md](../../RUBRIC.md): phần nào đã có kết quả, phần nào còn thiếu hoặc chưa kiểm được. Không tự nhận hoàn thành bước chưa chạy.

## 8. Công cụ AI và tài liệu tham khảo

Ghi công cụ đã dùng, dùng ở bước nào và phần đã tự kiểm tra; không dùng thì ghi rõ. Liệt kê nguồn ngoài repo được sử dụng. Không ghi mật khẩu/token hoặc đính kèm gold train.
