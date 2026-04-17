# Fine-Tune-PhoBERT-for-Classification
FineTune PhoBERT Base cho bài toán phân loại bình luận tích cực (1) tiêu cực (0) trên các sàn thương mại điện tử.

Dự án sử dụng PhoBERT Base để giải quyết bài toán phân loại cảm xúc bình luận trên các sàn thương mại điện tử:

0: Tiêu cực
1: Tích cực
 Data Preprocessing

Dữ liệu được tiền xử lý nhằm giảm nhiễu nhưng vẫn giữ lại thông tin quan trọng cho bài toán sentiment:

Chuẩn hóa chữ thường (lowercase)
Chuẩn hóa ký tự lặp (ví dụ: đẹpppp → đẹp)
Xóa emoji và ký tự đặc biệt
Chuẩn hóa khoảng trắng

 Không sử dụng stopword removal
Lý do: các từ như "rất", "cũng", "quá" mang ý nghĩa quan trọng trong việc xác định cảm xúc.

 Thay vào đó:

Loại bỏ các ký tự nhiễu (àáạảãâầấ...) để giảm kích thước và noise của dữ liệu
 Data Imbalance
Dữ liệu bị lệch lớp:
Class 0 (tiêu cực) chiếm ~ 1.5 lần class 1
Mục tiêu chính: tăng khả năng phát hiện class 1 (tích cực)
<img width="304" height="336" alt="image" src="https://github.com/user-attachments/assets/359b7994-18aa-4a92-9131-ef31480a06d9" />

 Giải pháp:

Sử dụng threshold tuning thay vì mặc định 0.5
Threshold tối ưu được chọn: 0.3

<img width="542" height="108" alt="image" src="https://github.com/user-attachments/assets/9ff1bce6-8874-4a4d-82b7-1a8a568880af" />

 Sequence Length Selection

Do độ dài các bình luận khác nhau:

Tiến hành phân tích phân phối độ dài token
<img width="581" height="456" alt="image" src="https://github.com/user-attachments/assets/c6cbcc24-9514-4c7c-aa98-246e25874261" />

Chọn:

 Max sequence length = 150

→ Đảm bảo cân bằng giữa:

Giữ đủ thông tin
Tối ưu tài nguyên tính toán
 Model Training
 Quá trình huấn luyện
Model bắt đầu hội tụ từ epoch 2–3
Sau đó xuất hiện dấu hiệu overfitting
 Kỹ thuật giảm overfitting

Áp dụng các phương pháp regularization:

hidden_dropout
attention_dropout
L2 Regularization
Learning rate scheduler:
Giúp ổn định quá trình học
Giảm hiện tượng vanishing gradient
Cải thiện khả năng exploration
 Model Performance

Kết quả cuối cùng:

Train Loss ≈ 0.11
Validation Loss ≈ 0.11
Test Loss ≈ 0.11
Threshold = 0.3
 <img width="581" height="456" alt="image" src="https://github.com/user-attachments/assets/98d3f88d-ce42-4498-b899-7b5c258aebdc" />

 Model đạt được:

Khả năng tổng quát tốt
Hạn chế overfitting
Cải thiện phát hiện class tích cực

Ma trận nhầm lẫn: 
<img width="316" height="299" alt="image" src="https://github.com/user-attachments/assets/ff8b5e86-e27a-4664-95bd-cedf7d4b5e72" />

 Kết luận

Mô hình fine-tune từ PhoBERT cho thấy hiệu quả tốt trong bài toán phân loại cảm xúc tiếng Việt, đặc biệt khi:

Xử lý dữ liệu phù hợp với ngữ cảnh tiếng Việt
Không loại bỏ các từ mang sắc thái cảm xúc
Điều chỉnh threshold để xử lý imbalance

