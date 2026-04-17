# Fine-Tune PhoBERT for Sentiment Classification

##  Giới thiệu
Dự án sử dụng PhoBERT Base để giải quyết bài toán **phân loại cảm xúc bình luận** trên các sàn thương mại điện tử:

- **0**: Tiêu cực  
- **1**: Tích cực  

---

##  Data Preprocessing
Dữ liệu được tiền xử lý nhằm giảm nhiễu nhưng vẫn giữ lại thông tin quan trọng cho bài toán sentiment:

- Chuẩn hóa chữ thường (*lowercase*)
- Chuẩn hóa ký tự lặp *(đẹpppp → đẹp)*
- Xóa emoji và ký tự đặc biệt
- Chuẩn hóa khoảng trắng  

###  Không sử dụng stopword removal
Lý do: các từ như **"rất"**, **"cũng"**, **"quá"** mang ý nghĩa quan trọng trong việc xác định cảm xúc.

###  Thay vào đó:
- Loại bỏ các ký tự nhiễu *(àáạảãâầấ...)* để giảm noise

---

##  Data Imbalance
- Class **0 (tiêu cực)** chiếm ~ **1.5 lần** class 1  
- Mục tiêu: **tăng khả năng phát hiện class 1**

<p align="center">
  <img src="https://github.com/user-attachments/assets/359b7994-18aa-4a92-9131-ef31480a06d9" width="300"/>
</p>

###  Giải pháp
- Sử dụng **threshold tuning** thay vì mặc định 0.5  
- Threshold tối ưu: **0.3**

<p align="center">
  <img src="https://github.com/user-attachments/assets/9ff1bce6-8874-4a4d-82b7-1a8a568880af" width="500"/>
</p>

---

##  Sequence Length Selection
- Phân tích phân phối độ dài token:

<p align="center">
  <img src="https://github.com/user-attachments/assets/c6cbcc24-9514-4c7c-aa98-246e25874261" width="500"/>
</p>

###  Lựa chọn:
- **Max sequence length = 150**

 Đảm bảo:
- Giữ đủ thông tin  
- Tối ưu tài nguyên  

---

##  Model Training

###  Quá trình huấn luyện
- Hội tụ từ **epoch 2–3**
- Sau đó xuất hiện **overfitting**
<img width="576" height="455" alt="image" src="https://github.com/user-attachments/assets/68f745e0-51f9-48ac-9962-f8517b6793c3" />

###  Regularization
- `hidden_dropout`
- `attention_dropout`
- **L2 Regularization**
- **Learning rate scheduler**

 Giúp:
- Ổn định training  
- Giảm vanishing gradient  
- Cải thiện exploration  

---

##  Model Performance

###  Kết quả
- **Train Loss ≈ 0.11**
- **Validation Loss ≈ 0.11**
- **Test Loss ≈ 0.11**
- **Threshold = 0.3**

###  Đánh giá
- Generalize tốt  
- Hạn chế overfitting  
- Tăng recall cho class tích cực  

---

##  Confusion Matrix
<p align="center">
  <img src="https://github.com/user-attachments/assets/ff8b5e86-e27a-4664-95bd-cedf7d4b5e72" width="320"/>
</p>

---

##  Kết luận
Mô hình fine-tune PhoBERT hoạt động hiệu quả cho bài toán sentiment tiếng Việt:

- Giữ lại từ mang sắc thái cảm xúc  
- Xử lý tốt dữ liệu imbalance  
- Threshold tuning cải thiện performance  
