# 2.5D Path Planning & BenchNav Suite

Chương trình đánh giá 4 thuật toán quy hoạch quỹ đạo 2.5D (`T-Hybrid A*`, `2.5D RRT*`, `2.5D Field D*`, `2.5D LazyPRM*`) trên bản đồ độ cao `.npz` / `.pt` / `.npy`.

## Cách chạy

1. **Menu tương tác chọn thuật toán và bản đồ trong `map_data/`:**
   ```bash
   python3 main.py
   # hoặc chỉ định trực tiếp map & thuật toán:
   python3 interactive_ui.py --map 000000 --algo 5
   ```

2. **Chạy benchmark cả 4 thuật toán qua CLI trên map có sẵn hoặc map `.npz` từ `rellis_25d_generator`:**
   ```bash
   python3 run_benchmark.py --map 000000 --start 8 14 --goal 34 25.5 --no-semantic --runs 5 --seed 42
   python3 run_benchmark.py --map ../rellis_25d_generator/output_25d/00000_000001_25d_dataset.npz --start 8 14 --goal 34 25.5 --runs 5 --seed 42
   python3 run_benchmark.py --map ../rellis_25d_generator/samples/00000_000500_25d_dataset.npz --start 10 15 --goal 22.2 18.4 --runs 5 --seed 42
   ```

   Dùng cờ `--no-semantic` để tắt dung hợp ngữ nghĩa (ablation Geometry-Only). Kết quả `.json` và ảnh so sánh `.png` được lưu vào `../../4. logs/`.

