# nail-recognition
# Đồ án: Nhận diện Móng tay (Nail Recognition) 💅

## 📌 Tiến độ Tuần 1: Chuẩn bị dữ liệu

### 1. Minh họa các lớp dữ liệu
Dưới đây là hình ảnh thực tế nhóm đã thu thập:

| Painted_Nail | Long_Nail | Short_Nail |
| :---: | :---: | :---: |
| <img src="painted_nail.jpg" width="250"> | <img src="long_nail.PNG" width="250"> | <img src="short_nail.JPG" width="250"> |

### 2. Thống kê
- **Tổng số ảnh:** 248 ảnh.
- **Tập Train:** 188 ảnh.
- **Tập Val:** 60 ảnh.

### 3. Mã nguồn
Chi tiết xử lý dữ liệu: [Tuan1_ChuanBiDuLieu.ipynb](Tuan1_ChuanBiDuLieu.ipynb)

### 🚧 Tiến độ Tuần 2: Xây dựng Model & Huấn luyện bước đầu
- Thiết lập kiến trúc mô hình Transfer Learning với **MobileNetV2** (đóng băng trọng số gốc).
- Áp dụng Data Augmentation để xử lý vấn đề thiếu hụt dữ liệu (248 ảnh).
- Đã tiến hành chạy huấn luyện lần 1 (Initial Training - 30 epochs) để lấy mức cơ sở (Baseline).
- **Kết quả bước đầu:** Mô hình (best_model.h5) đạt Accuracy tập kiểm định (Val) ở mức **~74%**.
- Chi tiết mã nguồn và biểu đồ: [Tuan2_HuanLuyenMoHinh.ipynb](Tuan2_HuanLuyenMoHinh.ipynb)

- ## 🚀 Tiến độ & Thành quả (Tuần 3)
Nhóm đã hoàn thành giai đoạn tối ưu hóa mô hình với các kết quả cụ thể:

- **Kỹ thuật:** Fine-tuning MobileNetV2 (mở khóa 25 lớp cuối).
- **Tham số:** Learning Rate $10^{-5}$, Optimizer Adam, huấn luyện 30 Epochs.
- **Dữ liệu:** 248 ảnh (188 Train / 60 Val) kết hợp kỹ thuật Data Augmentation.
- **Hiệu năng:** - Accuracy tập Train đạt **~90.3%**.
  - Accuracy tập Validation đạt **~56.5%**.
  - `Val_Loss` giảm từ 1.23 xuống **1.03**, cải thiện đáng kể hiện tượng Overfitting.
- **Thực tế:** Mô hình dự đoán chính xác ảnh chụp môi trường ngoài với độ tin cậy **55.14%** cho lớp `Short_Nail`.

### Mô hình CNN tự build
### 1. Mục tiêu
- Xây dựng mô hình CNN để phân loại ảnh móng tay thành 3 lớp: Long_Nail, Painted_Nail, Short_Nail;
- triển khai pipeline từ tiền xử lý dữ liệu → huấn luyện → đánh giá → suy luận
### 2. Công việc đã hoàn thành
- Tổ chức dataset theo cấu trúc Train/Val
- Viết hàm đọc ảnh bằng OpenCV, resize về (150,150)
- Vẽ phân phối lớp (bar/pie), hiển thị mẫu ảnh
- CNN tự xây dựng gồm 3 block Conv2D + MaxPooling, Fully Connected + Dropout
- Áp dụng Data Augmentation
- Vẽ biểu đồ đánh giá
- Tính Confusion Matrix và Classification Report
- Dự đoán 1 ảnh và batch ảnh
### 3. Kết quả
- huấn luyện và đánh giá chạy thành công
- có khả năng dự đoán nhưng độ chính xác chưa ổn định
- Xuất hiện hiện tượng: Dự đoán lệch về một lớp (Short_Nail), Confidence thấp (<50%)
## 📂 Sản phẩm bàn giao
- File mô hình: `nail_model_finetuned.h5`, `nail_cnn_model.keras`
- Notebook huấn luyện: `HUIT_NailRecognition_V2_FineTuning.ipynb`, `Tuan3_HuanLuyenMoHinhCNN.ipynb`

## 👥 Thành viên thực hiện
- Phạm Thanh Huy(HUIT)
- Lê Huỳnh Bảo Long
