# RELLIS-3D to 2.5D `.npz` Dataset Generator

Chương trình độc lập dùng để chuyển đổi dữ liệu LiDAR Ouster OS1-64 (`.bin`) và nhãn ngữ nghĩa SemanticKITTI (`.label`) từ **RELLIS-3D** thành các bản đồ 2.5D đa tầng định dạng `.npz`.

Được viết lại có module hóa và sửa lỗi từ mã nguồn tham khảo [Jikay71](https://github.com/Jikay71/Building-a-2.5D-Dataset-from-RELLIS-3D-for-Path-Planning), đồng thời hỗ trợ chế độ tái tạo schema Nhóm 1 (`--mode group1`).

## Cấu trúc thư mục

- `config.py`: Bảng tên/màu 20 lớp RELLIS-3D, bảng chi phí ngữ nghĩa và tham số lưới ROI / thân xe.
- `data_loader.py`: Trình đọc dữ liệu thô `.bin` & `.label` (hỗ trợ đa sequence `.zip`, thư mục giải nén, và `--download-sample` từ link Google Drive chính thức trên `unmannedlab/RELLIS-3D`).
- `pipeline.py`: Quy trình 3 bước vector hóa NumPy/SciPy + tìm đường A* (`jikay` & `group1`).
- `visualizer.py`: Xuất ảnh Dashboard 6 góc nhìn (`.png`) và ảnh động (`.gif`).
- `evaluator.py`: Kiểm tra cấu trúc `.npz` (`--inspect`) và tính chỉ số an toàn/cost (`--evaluate`).
- `main.py`: Giao diện dòng lệnh hợp nhất.

## Cài đặt thư viện

```bash
pip install -r requirements.txt
```

## Chuẩn bị dữ liệu thô

- **Cách 1 (Khuyên dùng để test nhanh 8 frames chính thức):**
  ```bash
  python3 main.py --download-sample
  ```
  Dữ liệu được tải về `raw_data/Rellis_3D_lidar_example/` gồm `000000`–`000007` (`.bin` và `.label`).

- **Cách 2 (Dữ liệu đầy đủ 5 sequences `00000`–`00004`):**
  Tải `Rellis_3D_os1_cloud_node_kitti_bin.zip` và `Rellis_3D_os1_cloud_node_semantickitti_label_id_20210614.zip` từ [unmannedlab/RELLIS-3D](https://github.com/unmannedlab/RELLIS-3D) và đặt vào `raw_data/` (hoặc chỉ định bằng `--data-dir /path/to/rellis`).

## Cách chạy

1. **Sinh toàn bộ bản đồ `.npz` theo pipeline Jikay71 (mặc định, lưới `200x150`):**
   ```bash
   python3 main.py --all --preview-every 1
   ```

2. **Sinh bản đồ `.npz` theo chuẩn Nhóm 1 (lưới `150x200`, Z phân vị 25%, `valid_mask`):**
   ```bash
   python3 main.py --mode group1 --all --preview-every 1 --out output_25d_group1
   ```

3. **Kiểm tra nội dung một file `.npz`:**
   ```bash
   python3 main.py --inspect output_25d/00000_000001_25d_dataset.npz
   ```

4. **Đánh giá định lượng toàn thư mục kết quả:**
   ```bash
   python3 main.py --evaluate --out output_25d
   ```

5. **Đưa file `.npz` vừa sinh (hoặc mẫu `samples/00000_000500_25d_dataset.npz`) sang chương trình Benchmark (tự động chuẩn hóa hệ trục về lưới chuẩn `150x200`):**
   ```bash
   python3 ../path_planning_benchmark/run_benchmark.py \
       --map ../rellis_25d_generator/output_25d/00000_000001_25d_dataset.npz \
       --start 8 14 --goal 34 25.5 --runs 5 --seed 42
   ```

