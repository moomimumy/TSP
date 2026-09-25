# TSP Heuristics — Discrete Mathematics Project

Dự án môn Toán rời rạc (Discrete Mathematics) giải bài toán **Người bán hàng du lịch (Traveling Salesman Problem - TSP)** bằng các thuật toán heuristic/local search, đánh giá và so sánh hiệu quả trên nhiều bộ dữ liệu chuẩn TSPLIB có kích thước khác nhau (từ 14 đến hơn 7000 thành phố).

## 📋 Mục tiêu

- Đọc và xử lý dữ liệu TSP theo định dạng chuẩn `.tsp` (TSPLIB), gồm cả dạng tọa độ (coordinate-based) và dạng ma trận khoảng cách cho sẵn (EXPLICIT).
- Cài đặt các thuật toán khởi tạo tour (constructive heuristics):
  - **Farthest Insertion**
  - **Nearest Neighbor**
- Cài đặt các thuật toán cải thiện tour (local search):
  - **Swap** (dựa trên danh sách lân cận + khởi động lại bằng Double Bridge)
  - **2-opt** (dựa trên danh sách lân cận + khởi động lại bằng Double Bridge)
- Kết hợp 4 pipeline (constructive + local search) và so sánh hiệu quả:
  - Farthest Insertion + Swap
  - Farthest Insertion + 2-opt
  - Nearest Neighbor + Swap
  - Nearest Neighbor + 2-opt
- Đánh giá kết quả bằng **gap (%)** so với lời giải tối ưu đã biết (known optimal), kiểm tra tính hợp lệ của tour, và trực quan hóa hiệu quả theo kích thước bài toán.

## 🗂️ Bộ dữ liệu

Sử dụng các bộ dữ liệu chuẩn từ **TSPLIB**, đặt trong thư mục Google Drive (`/content/drive/MyDrive/datasets/`):

| File | Số thành phố (N) | Giá trị tối ưu |
|---|---|---|
| burma14.tsp | 14 | 3323 |
| gr17.tsp | 17 | 2085 |
| lin105.tsp | 105 | 14379 |
| fl417.tsp | 417 | 11861 |
| u574.tsp | 574 | 36905 |
| pr1002.tsp | 1002 | 259045 |
| d2103.tsp | 2103 | 80450 |
| pla7397.tsp | 7397 | 23260728 |

> **Lưu ý:** File dữ liệu `.tsp` không được đính kèm trong repo này. Bạn cần tải các file tương ứng từ [TSPLIB](http://comopt.ifi.uni-heidelberg.de/software/TSPLIB95/) và đặt vào đúng thư mục, hoặc sửa lại biến `DATASET_DIR` trong notebook cho phù hợp với môi trường của bạn.

## ⚙️ Quy trình thực hiện

1. **Đọc dữ liệu** — Hàm `read_tsp()` phân tích file `.tsp`, trích xuất tọa độ hoặc ma trận khoảng cách, loại định dạng khoảng cách (`edge_weight_type`, `edge_weight_format`).
2. **Xây dựng ma trận khoảng cách** — Hàm `build_dist_matrix()` chuyển dữ liệu thô thành ma trận khoảng cách N×N, xử lý cả 2 trường hợp EXPLICIT và tính theo tọa độ.
3. **Khởi tạo tour ban đầu**
   - *Farthest Insertion*: chọn điểm bắt đầu đa dạng dựa trên convex hull, sau đó chèn thành phố xa nhất vào vị trí tốt nhất.
   - *Nearest Neighbor*: xây tour bằng cách liên tục ghé thăm thành phố gần nhất chưa đi qua.
4. **Cải thiện tour (local search)**
   - *Swap*: hoán đổi cặp thành phố dựa trên danh sách lân cận gần nhất, kết hợp Double Bridge để thoát cực trị địa phương.
   - *2-opt*: đảo đoạn (segment reversal) giữa các cặp cạnh để giảm tổng khoảng cách, kết hợp Double Bridge.
5. **Kiểm định (validation)** — Đảm bảo mỗi tour tạo ra hợp lệ (đi qua tất cả thành phố đúng một lần).
6. **So sánh & đánh giá** — Tính gap (%) so với giá trị tối ưu, so sánh giữa các pipeline, chạy nhiều seed ngẫu nhiên để lấy kết quả tốt nhất/trung bình.
7. **Trực quan hóa** — Vẽ biểu đồ thể hiện gap của từng pipeline theo kích thước bài toán (N).

## 🛠️ Công nghệ sử dụng

- Python 3
- numpy
- scipy (`scipy.spatial.distance`, `scipy.spatial.ConvexHull`)
- matplotlib

## 🚀 Cách chạy

### Trên Google Colab (khuyến nghị, vì notebook dùng Google Drive để lưu dữ liệu)
1. Upload các file `.tsp` cần dùng lên Google Drive, đường dẫn `MyDrive/datasets/`.
2. Mở notebook `project_discrete_maths-2.ipynb` trên [Google Colab](https://colab.research.google.com/).
3. Chạy cell đầu tiên để mount Google Drive, sau đó chạy lần lượt các cell còn lại.

### Chạy cục bộ (local)
1. Clone repo:
   ```bash
   git clone <link-repo-cua-ban>
   cd <ten-repo>
   ```
2. Cài đặt thư viện cần thiết:
   ```bash
   pip install numpy scipy matplotlib jupyter
   ```
3. Bỏ/thay đoạn code mount Google Drive, cập nhật `DATASET_DIR` trỏ tới thư mục chứa các file `.tsp` trên máy của bạn.
4. Mở và chạy notebook:
   ```bash
   jupyter notebook project_discrete_maths-2.ipynb
   ```

## 📊 Kết quả

Notebook xuất ra:
- Bảng thông tin từng bộ dữ liệu (kích thước, loại, giá trị tối ưu).
- Bảng so sánh gap (%) giữa Farthest Insertion và Farthest Insertion + Swap.
- Bảng so sánh gap (%) của 4 pipeline: FI+Swap, FI+2opt, NN+Swap, NN+2opt, cùng kết quả tốt nhất cho từng bộ dữ liệu.
- Biểu đồ trực quan thể hiện xu hướng gap khi kích thước bài toán (N) tăng lên, giúp đánh giá khả năng mở rộng (scalability) của từng phương pháp.

## 📄 License

Bạn có thể bổ sung license phù hợp cho repo (ví dụ MIT) nếu muốn chia sẻ công khai.
