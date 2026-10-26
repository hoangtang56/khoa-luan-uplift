# Khóa luận về mô hình hóa hiệu ứng tăng thêm

Mã và kết quả thực nghiệm phục vụ khóa luận về lựa chọn khách hàng cho quảng cáo trực tuyến trên bộ dữ liệu Criteo Uplift Modeling Dataset v2.

## Nội dung

- `EDA.ipynb`: kiểm tra và phân tích khám phá dữ liệu.
- `Uplift_Modeling_CriteoV2_Colab_T4_2M (1) (1).ipynb`: sổ tay huấn luyện và đánh giá mô hình.
- `uplift_modelling_colab_2m/outputs_full_consistent_v2/`: cấu hình, bảng kết quả, biểu đồ, mô hình đã huấn luyện và dữ liệu dự đoán.

## Sử dụng

1. Mở sổ tay bằng Google Colab.
2. Chuẩn bị bộ dữ liệu Criteo v2 từ nguồn phát hành chính thức; dữ liệu gốc không được kèm trong kho này.
3. Kiểm tra và điều chỉnh đường dẫn dữ liệu, Google Drive và thư mục kết quả trong sổ tay theo tài khoản sử dụng.
4. Đọc các bước cài đặt trong sổ tay và chạy lần lượt các ô mã. Quá trình huấn luyện trên dữ liệu đầy đủ cần tài nguyên tính toán phù hợp.

Cấu hình và phiên bản thư viện của lần chạy đã lưu được ghi trong `run_config.json` và `tables/environment_versions.csv`. Tên thư mục cũ có chứa `2m`; cấu hình đã lưu ghi nhận 9.786.802 quan sát huấn luyện và mẫu con 200.000 quan sát dành cho rừng nhân quả.

## Phạm vi kết quả

Thực nghiệm so sánh ba phương pháp học nền, rừng nhân quả, mô hình dự đoán phản hồi và lựa chọn ngẫu nhiên. Kết quả hỗ trợ đánh giá lựa chọn khách hàng theo tỷ lệ tiếp cận; chưa xác định ngân sách hay lợi nhuận tối ưu.

Các tệp mô hình và dự đoán là kết quả của lần chạy đã lưu. Khi chạy lại, hãy kiểm tra cấu hình và đối chiếu kết quả. Dữ liệu Criteo tuân theo điều kiện sử dụng của bên phát hành.
