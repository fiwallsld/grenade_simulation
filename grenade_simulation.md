# Ứng Dụng Mô Phỏng Vị Trí Ném Lựu Đạn Bằng Python

---

## 1. Phân Tích Ý Tưởng

### 1.1. Mục Tiêu
- Xây dựng một ứng dụng desktop mô phỏng quỹ đạo ném lựu đạn dựa trên dữ liệu từ camera.
- Phân tích và dự đoán vị trí ném lựu đạn ngay cả khi khói bụi che khuất tầm nhìn.

### 1.2. Các Thành Phần Chính
1. **Thu thập dữ liệu từ camera**:
   - Ghi lại video hoặc hình ảnh từ camera để theo dõi chuyển động của lựu đạn.
2. **Xử lý hình ảnh**:
   - Phát hiện và theo dõi lựu đạn trong video.
   - Xử lý khói bụi để ước lượng vị trí cuối cùng.
3. **Mô phỏng quỹ đạo**:
   - Sử dụng các phương trình vật lý để tính toán quỹ đạo dựa trên dữ liệu thu thập được.
4. **Giao diện người dùng**:
   - Hiển thị kết quả mô phỏng và quỹ đạo lựu đạn.

---

## 2. Công Cụ và Ngôn Ngữ
- **Ngôn ngữ lập trình**: Python.
- **Thư viện chính**:
  - **OpenCV**: Xử lý hình ảnh và video.
  - **NumPy**: Tính toán số học.
  - **Matplotlib**: Vẽ đồ thị quỹ đạo.
  - **PyQt5**: Tạo giao diện người dùng.
  - **SciPy**: Tính toán vật lý và toán học.

---

## 3. Các Bước Thực Hiện

### 3.1. Thiết Lập Môi Trường
1. **Cài đặt Python**:
   - Tải và cài đặt Python từ [python.org](https://www.python.org/).
2. **Cài đặt thư viện**:
   - Mở terminal và chạy các lệnh sau:
     ```bash
     pip install opencv-python numpy matplotlib PyQt5 scipy
     ```

---

### 3.2. Thu Thập Dữ Liệu Từ Camera
1. **Kết nối camera**:
   ```python
   import cv2

   cap = cv2.VideoCapture(0)  # 0 là ID của camera mặc định
   if not cap.isOpened():
       print("Không thể kết nối camera!")
       exit()
2. **Ghi lại video**:
   ```python
   fourcc = cv2.VideoWriter_fourcc(*'XVID')
   out = cv2.VideoWriter('output.avi', fourcc, 20.0, (640, 480))

   while True:
      ret, frame = cap.read()
      if not ret:
         print("Không thể nhận khung hình!")
         break

      out.write(frame)  # Ghi khung hình vào video
      cv2.imshow('Camera', frame)  # Hiển thị khung hình

      if cv2.waitKey(1) == ord('q'):  # Nhấn 'q' để thoát
         break

   cap.release()
   out.release()
   cv2.destroyAllWindows()
   ```
### 3.3. Xử Lý Hình Ảnh và Theo Dõi Lựu Đạn
1. **Phát hiện lựu đạn**:
   ```python
   import cv2
   import numpy as np

   def detect_grenade(frame):
      hsv = cv2.cvtColor(frame, cv2.COLOR_BGR2HSV)
      lower_green = np.array([35, 100, 100])
      upper_green = np.array([85, 255, 255])
      mask = cv2.inRange(hsv, lower_green, upper_green)
      contours, _ = cv2.findContours(mask, cv2.RETR_TREE, cv2.CHAIN_APPROX_SIMPLE)
      if contours:
         largest_contour = max(contours, key=cv2.contourArea)
         (x, y, w, h) = cv2.boundingRect(largest_contour)
         return (x + w // 2, y + h // 2)  # Trả về tâm của lựu đạn
      return None
   ```
2. **Theo dõi quỹ đạo**:
   ```python
   positions = []

   while True:
      ret, frame = cap.read()
      if not ret:
         break

      position = detect_grenade(frame)
      if position:
         positions.append(position)
         cv2.circle(frame, position, 5, (0, 255, 0), -1)  # Vẽ điểm trên khung hình

      cv2.imshow('Tracking', frame)
      if cv2.waitKey(1) == ord('q'):
         break
### 3.4. Mô Phỏng Quỹ Đạo
1. **Tính toán quỹ đạo**:
   ```python
   import numpy as np
   from scipy.optimize import curve_fit

   def parabolic_trajectory(x, a, b, c):
      return a * x**2 + b * x + c

   x = np.array([p[0] for p in positions])
   y = np.array([p[1] for p in positions])

   params, _ = curve_fit(parabolic_trajectory, x, y)
2. **Vẽ quỹ đạo**:
   ```python
   import matplotlib.pyplot as plt

   plt.scatter(x, y, color='red', label='Vị trí thực tế')
   x_fit = np.linspace(min(x), max(x), 100)
   y_fit = parabolic_trajectory(x_fit, *params)
   plt.plot(x_fit, y_fit, label='Quỹ đạo mô phỏng')

   plt.xlabel('X')
   plt.ylabel('Y')
   plt.legend()
   plt.show()
### 3.5. Tạo Giao Diện Người Dùng
1. **Thiết kế giao diện**:
   ```python
      from PyQt5.QtWidgets import QApplication, QMainWindow, QLabel, QVBoxLayout, QWidget
      from PyQt5.QtGui import QImage, QPixmap
      import sys

      class MainWindow(QMainWindow):
         def __init__(self):
            super().__init__()
            self.setWindowTitle("Mô phỏng ném lựu đạn")
            self.setGeometry(100, 100, 800, 600)

            self.image_label = QLabel(self)
            self.image_label.resize(640, 480)

            layout = QVBoxLayout()
            layout.addWidget(self.image_label)

            container = QWidget()
            container.setLayout(layout)
            self.setCentralWidget(container)

         def update_image(self, frame):
            height, width, channel = frame.shape
            bytes_per_line = 3 * width
            q_img = QImage(frame.data, width, height, bytes_per_line, QImage.Format_RGB888)
            self.image_label.setPixmap(QPixmap.fromImage(q_img))

      app = QApplication(sys.argv)
      window = MainWindow()
      window.show()
      sys.exit(app.exec_())
2. **Kết hợp xử lý hình ảnh và giao diện**:
   ```python
      def main():
         cap = cv2.VideoCapture(0)
         app = QApplication(sys.argv)
         window = MainWindow()

         while True:
            ret, frame = cap.read()
            if not ret:
                  break

            window.update_image(cv2.cvtColor(frame, cv2.COLOR_BGR2RGB))
            app.processEvents()

         cap.release()

      if __name__ == "__main__":
         main()
### 3.6. Tối Ưu Hóa và Triển Khai
1. **Tối ưu hóa**:

Sử dụng đa luồng để xử lý camera và giao diện đồng thời.

Tối ưu thuật toán phát hiện và theo dõi đối tượng.

Đóng gói ứng dụng:

Sử dụng PyInstaller để đóng gói ứng dụng:
   ```bash
      pip install pyinstaller
      pyinstaller --onefile --windowed your_script.py
   ```
## 4. Kết Luận
Ứng dụng mô phỏng quỹ đạo ném lựu đạn bằng Python kết hợp xử lý hình ảnh, tính toán vật lý, và giao diện người dùng. Với các bước trên, bạn có thể xây dựng một ứng dụng hoàn chỉnh và tối ưu hóa để sử dụng trong thực tế.