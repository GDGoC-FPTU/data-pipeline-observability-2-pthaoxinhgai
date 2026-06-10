# Báo Cáo Thí Nghiệm: Ảnh Hưởng Của Chất Lượng Dữ Liệu Đến AI Agent

**Mã số sinh viên:** AI20K-XXXX
**Tên:** pthaoxinhgai
**Ngày thực hiện:** 2026-06-10

---

## 1. Kết Quả Thí Nghiệm

Mình chạy `agent_simulation.py` với 2 bộ dữ liệu: clean data được tạo từ ETL pipeline (`processed_data.csv`) và garbage data được tạo bằng `generate_garbage.py`.

| Kịch bản | Phản hồi của Agent | Độ chính xác (1-10) | Ghi chú |
|----------|--------------------|---------------------|---------|
| Clean Data (`processed_data.csv`) | Agent: Based on my data, the best choice is Laptop at $1200. | 9 | Dữ liệu đã được validate, category được chuẩn hóa đúng định dạng Title Case nên agent lọc được electronics và chọn sản phẩm có giá cao nhất một cách hợp lý. |
| Garbage Data (`garbage_data.csv`) | Agent: Based on my data, the best choice is Nuclear Reactor at $999999. | 2 | Dữ liệu bị nhiễu bởi outlier rất lớn, trùng ID, giá trị thiếu và sai kiểu dữ liệu, khiến kết quả khuyến nghị bị lệch khỏi ngữ cảnh thực tế. |

---

## 2. Phân Tích Và Nhận Xét (Phan tich va nhan xet)

### Tại sao Agent trả lời sai khi dùng Garbage Data?

Agent trả lời sai khi dùng garbage data vì nó phụ thuộc trực tiếp vào dữ liệu đầu vào. Trong mô phỏng này, agent chỉ lọc các sản phẩm thuộc category electronics, sau đó chọn record có `price` cao nhất. Khi bộ dữ liệu có outlier như Nuclear Reactor với giá `999999`, giá trị này lớn hơn rất nhiều so với Laptop nên agent xem đó là lựa chọn tốt nhất, dù kết quả này không hợp lý trong ngữ cảnh mua sản phẩm điện tử thông thường. Ngoài ra, duplicate ID làm mất tính duy nhất của record, wrong data type như `"ten dollars"` có thể làm lỗi hoặc làm sai schema, còn null values khiến quá trình lọc category và xử lý price thiếu ổn định. Vì vậy, nếu pipeline không validate và transform dữ liệu trước, prompt tốt vẫn không đủ để đảm bảo agent trả lời đúng.

---

## 3. Kết Luận

**Quality Data > Quality Prompt?** Mình đồng ý. Prompt rõ ràng rất quan trọng, nhưng chất lượng dữ liệu vẫn là nền tảng. Nếu dữ liệu đầu vào bị lỗi, thiếu, sai kiểu hoặc có outlier bất thường, agent có thể suy luận dựa trên thông tin sai và đưa ra câu trả lời không đáng tin cậy. Một pipeline tốt cần kiểm tra, chuẩn hóa và ghi nhận trạng thái xử lý dữ liệu trước khi đưa dữ liệu đó cho agent sử dụng.
