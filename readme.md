# Ứng Dụng Mô Phỏng Vị Trí Ném Lựu Đạn Bằng Python

---

## Mô Tả Dự Án
Ứng dụng này được phát triển để mô phỏng quỹ đạo ném lựu đạn dựa trên dữ liệu từ camera. Ứng dụng sử dụng các kỹ thuật xử lý hình ảnh, tính toán vật lý, và giao diện người dùng để hiển thị quỹ đạo mô phỏng.

---

## Tính Năng Chính
- **Thu thập dữ liệu từ camera**: Ghi lại video và phát hiện lựu đạn.
- **Theo dõi quỹ đạo**: Xác định vị trí lựu đạn qua từng khung hình.
- **Mô phỏng quỹ đạo**: Tính toán và hiển thị quỹ đạo dựa trên các phương trình vật lý.
- **Giao diện người dùng**: Hiển thị video và quỹ đạo mô phỏng.

---

## Công Nghệ Sử Dụng
- **Ngôn ngữ lập trình**: Python.
- **Thư viện chính**:
  - OpenCV: Xử lý hình ảnh và video.
  - NumPy: Tính toán số học.
  - Matplotlib: Vẽ đồ thị quỹ đạo.
  - PyQt5: Tạo giao diện người dùng.
  - SciPy: Tính toán vật lý và toán học.

---

## Cài Đặt và Chạy Ứng Dụng

### Yêu Cầu Hệ Thống
- Python 3.7 trở lên.
- Camera hoặc video mẫu để thử nghiệm.

### Cài Đặt
1. Clone repository:
   ```bash
   git clone https://github.com/fiwallsld/grenade-simulation.git
   cd grenade-simulation
   ```

2. Cài đặt các thư viện cần thiết:
   ```bash
   pip install -r requirements.txt
   ```

### Chạy Ứng Dụng
Chạy file chính của ứng dụng:
   ```bash
   python main.py
   ```
Sử dụng giao diện để kết nối camera, theo dõi lựu đạn, và xem quỹ đạo mô phỏng.

---

## Cấu Trúc Thư Mục
```
grenade-simulation/
├── main.py                # File chính của ứng dụng
├── requirements.txt       # Danh sách các thư viện cần thiết
├── README.md              # Tài liệu hướng dẫn
├── data/                  # Thư mục chứa video và dữ liệu mẫu
├── utils/                 # Thư mục chứa các tiện ích hỗ trợ
│   ├── image_processing.py # Xử lý hình ảnh
│   ├── trajectory.py      # Tính toán quỹ đạo
└── ui/                    # Thư mục chứa giao diện người dùng
    └── main_window.py     # Giao diện chính
```

---

## Đóng Góp
Nếu bạn muốn đóng góp vào dự án, vui lòng làm theo các bước sau:

1. Fork repository.
2. Tạo một nhánh mới:
   ```bash
   git checkout -b feature/YourFeatureName
   ```
3. Commit các thay đổi:
   ```bash
   git commit -m 'Add some feature'
   ```
4. Push lên nhánh:
   ```bash
   git push origin feature/YourFeatureName
   ```
5. Tạo một Pull Request.

---

## Giấy Phép
Dự án này được phân phối dưới giấy phép MIT. Xem file LICENSE để biết thêm chi tiết.

---

## Liên Hệ
- **Tác giả**: Hoàng Thành
- **Email**: thanhpromu@gmail.com
- **GitHub**: [fiwallsld](https://github.com/fiwallsld)

---

## Ghi Chú
Dự án này được phát triển cho mục đích học tập và nghiên cứu.
Mọi đóng góp và phản hồi đều được hoan nghênh!
