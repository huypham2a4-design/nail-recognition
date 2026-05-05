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

---

### 🚧 Tiến độ Tuần 2: Xây dựng Model & Huấn luyện bước đầu
- Thiết lập kiến trúc mô hình Transfer Learning với **MobileNetV2** (đóng băng trọng số gốc).
- Áp dụng Data Augmentation để xử lý vấn đề thiếu hụt dữ liệu (248 ảnh).
- Đã tiến hành chạy huấn luyện lần 1 (Initial Training - 30 epochs) để lấy mức cơ sở (Baseline).
- **Kết quả bước đầu:** Mô hình (best_model.h5) đạt Accuracy tập kiểm định (Val) ở mức **~74%**.
- Chi tiết mã nguồn và biểu đồ: [Tuan2_HuanLuyenMoHinh.ipynb](Tuan2_HuanLuyenMoHinh.ipynb)

---

## 🚀 Tiến độ & Thành quả (Tuần 3)
Nhóm đã hoàn thành giai đoạn tối ưu hóa mô hình với các kết quả cụ thể:
- **Kỹ thuật:** Fine-tuning MobileNetV2 (mở khóa 25 lớp cuối).
- **Tham số:** Learning Rate $10^{-5}$, Optimizer Adam, huấn luyện 30 Epochs.
- **Dữ liệu:** 248 ảnh (188 Train / 60 Val) kết hợp kỹ thuật Data Augmentation.
- **Hiệu năng:**
  - Accuracy tập Train đạt **~90.3%**.
  - Accuracy tập Validation đạt **~56.5%**.
  - `Val_Loss` giảm từ 1.23 xuống **1.03**, cải thiện đáng kể hiện tượng Overfitting.
- **Thực tế:** Mô hình dự đoán chính xác ảnh chụp môi trường ngoài với độ tin cậy **55.14%** cho lớp `Short_Nail`.

### Mô hình CNN tự build
#### 1. Mục tiêu
- Xây dựng mô hình CNN để phân loại ảnh móng tay thành 3 lớp: Long_Nail, Painted_Nail, Short_Nail.
- Triển khai pipeline từ tiền xử lý dữ liệu → huấn luyện → đánh giá → suy luận.

#### 2. Công việc đã hoàn thành
- Tổ chức dataset theo cấu trúc Train/Val
- Viết hàm đọc ảnh bằng OpenCV, resize về (150,150)
- Vẽ phân phối lớp (bar/pie), hiển thị mẫu ảnh
- CNN tự xây dựng gồm 3 block Conv2D + MaxPooling, Fully Connected + Dropout
- Áp dụng Data Augmentation
- Vẽ biểu đồ đánh giá
- Tính Confusion Matrix và Classification Report
- Dự đoán 1 ảnh và batch ảnh

#### 3. Kết quả
- Huấn luyện và đánh giá chạy thành công
- Có khả năng dự đoán nhưng độ chính xác chưa ổn định
- Xuất hiện hiện tượng: Dự đoán lệch về một lớp (Short_Nail), Confidence thấp (<50%)

---

## ✨ Tiến độ & Thành quả (Tuần 4): Chuẩn hóa Dataset & Đột phá Độ chính xác

### 1. Phân tích vấn đề từ tuần 3

Sau khi phân tích kết quả tuần 3, nhóm xác định 2 nguyên nhân chính khiến độ chính xác thấp:

**Vấn đề 1 — Chất lượng ảnh kém:**
Ảnh trong dataset chụp cả ngón tay khiến móng chỉ chiếm 30-40% diện tích ảnh. Sử dụng kỹ thuật **Grad-CAM** để trực quan hóa vùng mà mô hình tập trung, nhóm phát hiện attention map phân tán khắp ảnh thay vì tập trung vào vùng móng — đây là nguyên nhân trực tiếp khiến mô hình học sai đặc trưng.

**Vấn đề 2 — Ảnh bị lẫn lộn giữa các class:**
Một số ảnh `Long_Nail` có móng sơn màu nude/hồng nhạt trông giống `Painted_Nail`, gây nhầm lẫn 11/25 ảnh (44%) ở tuần 3.

### 2. Giải pháp thực hiện

**2.1 Chuẩn hóa lại toàn bộ dataset**

Nhóm xây dựng tiêu chuẩn ảnh thống nhất và tiến hành lọc, chỉnh sửa lại toàn bộ dataset:

| Tiêu chí | Yêu cầu |
|---|---|
| Tỉ lệ khung hình | Móng chiếm 60-80% diện tích ảnh |
| Góc chụp | Thẳng từ trên xuống, vuông góc bề mặt móng |
| Nền ảnh | Đơn sắc trắng hoặc xám nhạt |
| Ánh sáng | Đều, không bóng đổ lên vùng móng |
| Loại bỏ | Ảnh móng sơn màu nude/hồng nhạt dễ nhầm với móng tự nhiên |

Sau khi lọc: **283 ảnh (188 Train / 73 Val)** — ít hơn tuần 3 nhưng chất lượng đồng đều hơn.

**2.2 Cải tiến kiến trúc mô hình**

- Bỏ Fine-tuning (phát hiện Fine-tuning làm xáo trộn weights đã học tốt từ Phase 1 do dataset nhỏ)
- Tăng Dropout từ 0.3 lên **0.5** ở lớp đầu, thêm Dropout **0.4** ở lớp thứ hai
- Giảm Dense layer từ 256 xuống **128 neurons**
- Thêm **L2 Regularization** (lambda=0.001)
- Đổi monitor từ `val_accuracy` sang `val_loss` để ổn định hơn
- Thêm callback **ReduceLROnPlateau** tự động giảm learning rate khi val_loss không cải thiện

### 3. Kết quả đạt được

| Chỉ số | Tuần 3 | Tuần 4 |
|---|---|---|
| Val Accuracy | 67.7% | **96%** |
| F1-Score | 0.36 | **0.96** |
| Train-Val Gap | ~17% | ~4% |
| Long→Painted nhầm | 11/25 ảnh | **0/20 ảnh** |
| Số ảnh sai / Val | 29/85 | **3/73** |

**Kết quả Confusion Matrix:**

| Class | Precision | Recall | F1-Score |
|---|---|---|---|
| Long_Nail | 1.00 | 0.90 | 0.95 |
| Painted_Nail | 1.00 | 0.96 | 0.98 |
| Short_Nail | 0.90 | 1.00 | 0.95 |
| **Trung bình** | **0.97** | **0.95** | **0.96** |

Mô hình chỉ sai 3/73 ảnh validation — đều là trường hợp biên khó phân biệt ngay cả với người quan sát thông thường.

### 4. Kết luận tuần 4

> Kết quả tuần 4 chứng minh **chất lượng dataset quan trọng hơn độ phức tạp của mô hình**. Với cùng kiến trúc MobileNetV2, chỉ cần chuẩn hóa lại ảnh đầu vào, accuracy tăng từ 67.7% lên **96%** — tăng 28.3% dù số lượng ảnh giảm từ 314 xuống 283.

### 5. Mã nguồn
Chi tiết: [nail_classification_tuan4.ipynb](nail_classification_tuan4.ipynb)

---

## 📂 Sản phẩm bàn giao
- File mô hình: `nail_model_finetuned.h5`, `nail_cnn_model.keras`, `best_model.keras`
- Notebook huấn luyện: `HUIT_NailRecognition_V2_FineTuning.ipynb`, `Tuan3_HuanLuyenMoHinhCNN.ipynb`, `nail_classification_tuan4.ipynb`

## 👥 Thành viên thực hiện
- Phạm Thanh Huy (HUIT)
- Lê Huỳnh Bảo Long
