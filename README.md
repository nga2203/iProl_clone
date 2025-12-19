[Mục lục]
> Mô hình được đề xuất nhằm giải quyết bài toán nhận dạng promoter DNA của vi khuẩn E. coli bằng cách kết hợp các mô hình học sâu hiện đại trong xử lý chuỗi dài. Trình tự DNA được xem như dữ liệu dạng văn bản, từ đó áp dụng các kỹ thuật học biểu diễn tương tự trong Xử lý Ngôn ngữ Tự nhiên (NLP).

Khung mô hình khai thác đồng thời:

Ngữ cảnh dài hạn thông qua mô hình Longformer được huấn luyện trước

Đặc trưng cục bộ bằng mạng CNN 1D

Phụ thuộc dài hạn hai chiều bằng BiLSTM

## Tổng quan
![iproL_new](images/iproL_new.png)
Hình 1. Tổng quan. Bao gồm (A) xây dựng dữ liệu, (B) kiểm định chéo năm lần (five-fold cross-validation), và (C) khung mô hình

## Bộ dữ liệu
Dữ liệu thực nghiệm được thu thập từ RegulonDB, một cơ sở dữ liệu công khai cung cấp thông tin về các yếu tố điều hòa gen và trình tự promoter của E. coli.

Loại dữ liệu: Chuỗi DNA (A, C, G, T)

Bài toán: Phân loại nhị phân (Promoter / Non-promoter)

Biểu diễn dữ liệu: k-mer với k = 2

Chiều dài chuỗi đầu vào: 81 bp

Các chuỗi DNA được chia thành các token 2-mer (stride = 1) và ánh xạ thành chuỗi token làm đầu vào cho mô hình Transformer. Dữ liệu được padding tự động để phù hợp với cơ chế attention của Longformer.

Nguồn dữ liệu có thể tải tại:
https://regulondb.ccg.unam.mx/index.jsp

## Mô hình Longformer được huấn luyện trước
Mô hình sử dụng Longformer-base-4096, một biến thể của Transformer được thiết kế để xử lý chuỗi dài với cơ chế sparse attention.

Model: longformer-base-4096

Chiều dài chuỗi tối đa: 4096 token

Kích thước embedding: 768 chiều

Đầu ra embedding: ma trận kích thước 79 × 768

Mô hình Longformer được sử dụng chỉ để trích xuất embedding, không thực hiện fine-tuning nhằm tránh overfitting do kích thước bộ dữ liệu huấn luyện hạn chế.
## Kiến trúc trích xuất đặc trưng

Sau lớp nhúng Longformer, mô hình sử dụng:

3 lớp CNN 1D để trích xuất đặc trưng cục bộ (local features)

1 lớp BiLSTM để học các phụ thuộc dài hạn hai chiều (global features)

2 lớp Fully Connected để đưa ra dự đoán cuối cùng

Đầu ra cuối cùng là xác suất thuộc lớp promoter thông qua hàm kích hoạt Sigmoid.
## Huấn luyện và đánh giá

Framework: PyTorch

Optimizer: Adam

Learning rate: 0.0005 (giảm theo StepLR)

Batch size: 32

Epochs: 250

Loss function: Cross Entropy

Mô hình được đánh giá bằng kiểm chứng chéo năm lần (five-fold cross-validation) với các chỉ số:
Accuracy, Sensitivity, Specificity, F1-score, MCC và ROC-AUC.


## Chạy chương trình
```bash
cd ~/Documents/pbc/iProL/src
nohup jupyter notebook > ../jupyter_log/jupyter.log 2>&1 &
nohup ipython -c "%run ./main_iProL.ipynb" > ../jupyter_log/out.txt 2>&1 &
