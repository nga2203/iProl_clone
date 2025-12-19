[Mục lục]
> Một công cụ dự đoán mới để xác định promoter DNA từ thông tin chuỗi dựa trên mô hình Longformer được huấn luyện trước

## Tổng quan
![iproL_new](images/iproL_new.png)
Hình 1. Tổng quan. Bao gồm (A) xây dựng dữ liệu, (B) kiểm định chéo năm lần (five-fold cross-validation), và (C) khung mô hình

## Bộ dữ liệu
Dữ liệu thực nghiệm của chúng tôi được lấy từ RegulonDB và sử dụng cùng bộ dữ liệu chuẩn (benchmark dataset) như BERT-Promoter. Bộ dữ liệu này ban đầu được cung cấp bởi iPSW(2L)-PseKNC, và dữ liệu hoàn chỉnh có thể được tải xuống từ https://regulondb.ccg.unam.mx/index.jsp.

## Mô hình Longformer được huấn luyện trước
Mô hình được huấn luyện trước mà chúng tôi sử dụng có tên là longformer-base-4096, hỗ trợ chuỗi văn bản dài tới 4096 token và có thể nhúng mỗi từ vào vector 768 chiều. Mô hình Longformer được huấn luyện trước này có thể được tải xuống từ HuggingFace, cụ thể tại https://huggingface.co/allenai/longformer-base-4096/tree/main.

## Chạy chương trình
```bash
cd ~/Documents/pbc/iProL/src
nohup jupyter notebook > ../jupyter_log/jupyter.log 2>&1 &
nohup ipython -c "%run ./main_iProL.ipynb" > ../jupyter_log/out.txt 2>&1 &
