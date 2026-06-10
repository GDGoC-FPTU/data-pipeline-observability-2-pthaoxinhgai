[![Open in Visual Studio Code](https://classroom.github.com/assets/open-in-vscode-2e0aaae1b6195c2367325f4f02e2d04e9abb55f0b24a779b69b11b9e10269abc.svg)](https://classroom.github.com/online_ide?assignment_repo_id=24112761&assignment_repo_type=AssignmentRepo)

# Lab Ngày 10: Data Pipeline & Data Observability

**Student Email:** hpthao15122004@gmail.com
**Name:** Hoàng Phương Thảo

---

## Mô Tả

Bài lab này xây dựng một ETL pipeline đơn giản cho dữ liệu sản phẩm. Pipeline đọc dữ liệu từ file `raw_data.json`, kiểm tra và loại bỏ các record không hợp lệ, chuẩn hóa dữ liệu, tính giá sau khi giảm 10%, thêm timestamp xử lý bằng cột `processed_at`, sau đó lưu kết quả ra file `processed_data.csv`.

Phần observability tập trung vào việc in log trong quá trình chạy pipeline, bao gồm số record đã đọc, số record hợp lệ, số record bị loại và số record được lưu. Phần stress test dùng một agent mô phỏng đơn giản để so sánh kết quả khi chạy với clean data và garbage data.

---

## Cách Chạy

### Cài đặt thư viện

```bash
pip install pandas pytest
```

### Chạy ETL Pipeline

```bash
python solution.py
```

Lệnh này tạo file `processed_data.csv`. Với dữ liệu mẫu hiện tại, pipeline đọc 5 record, giữ lại 3 record hợp lệ và thêm các cột `discounted_price`, `processed_at`.

### Chạy Agent Simulation

```bash
python generate_garbage.py
python agent_simulation.py
```

File `generate_garbage.py` tạo `garbage_data.csv` chứa các lỗi chất lượng dữ liệu như trùng ID, sai kiểu dữ liệu, outlier và giá trị thiếu. File `agent_simulation.py` so sánh phản hồi của agent khi dùng `processed_data.csv` và `garbage_data.csv`.

---

## Cấu Trúc Thư Mục

```text
solution.py              # Script ETL pipeline
processed_data.csv       # Kết quả sau khi xử lý dữ liệu
garbage_data.csv         # Dữ liệu lỗi dùng cho stress test
experiment_report.md     # Báo cáo thí nghiệm
README.md                # File hướng dẫn này
```

---

## Kết Quả

Pipeline đã loại bỏ 2 record lỗi: một record có `price` âm và một record có `category` rỗng. Với clean data, agent chọn Laptop giá `$1200`, đây là kết quả hợp lý trong nhóm electronics. Với garbage data, agent chọn Nuclear Reactor giá `$999999` vì bị ảnh hưởng bởi outlier, cho thấy chất lượng dữ liệu đầu vào ảnh hưởng trực tiếp đến câu trả lời của agent.
