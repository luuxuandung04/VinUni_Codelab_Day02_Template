# 01 - Problem Scan

## Phase 1 - SCAN: Tìm kiếm cơ hội AI

### Danh sách bài toán cá nhân

| # | Subsidiary | Lens | Mô tả ngắn bài toán |
|---|---|---|---|
| 1 | Xanh SM | Tốn thời gian | Điều phối viên mất nhiều thời gian xử lý khi tài xế báo pin yếu giữa đường: phải tra vị trí xe, tìm trạm sạc gần nhất, kiểm tra khoảng cách và soạn hướng dẫn thủ công. |
| 2 | Vinhomes | Lặp lại | Nhân viên CSKH phải phân loại thủ công các phản ánh của cư dân như mất nước, hỏng đèn, tiếng ồn, vệ sinh, thang máy để chuyển đến đúng bộ phận xử lý. |
| 3 | Vinmec | Tốn thời gian | Bác sĩ và điều dưỡng mất nhiều thời gian soạn tóm tắt hồ sơ xuất viện cho bệnh nhân từ bệnh án, xét nghiệm, chẩn đoán và chỉ định điều trị. |
| 4 | VinFast | AI-upgrade | Khách hàng mô tả lỗi xe bằng ngôn ngữ tự nhiên nhưng hệ thống chưa tự động phân loại được nhóm lỗi kỹ thuật ban đầu để chuyển đúng đội hỗ trợ. |
| 5 | Vinpearl | Stakeholder Pain | Quản lý khách sạn phải đọc thủ công nhiều review từ khách hàng để phát hiện các phàn nàn nghiêm trọng về phòng, vệ sinh, thái độ nhân viên hoặc dịch vụ. |

---

## Phase 2 - QUICK-ASSESS: 3 Quick Problem Cards

### Quick Problem Card #1

| Mục | Nội dung |
|---|---|
| Bài toán | Hỗ trợ điều phối viên Xanh SM xử lý tình huống tài xế báo pin yếu giữa đường. |
| Công ty thành viên | Xanh SM |
| Ai đang đau? | Tài xế phải chờ hướng dẫn, điều phối viên bị quá tải vào giờ cao điểm, khách hàng có thể bị trễ chuyến. |
| Workflow hiện tại | 1. Tài xế gọi tổng đài báo pin yếu. 2. Điều phối viên kiểm tra vị trí xe. 3. Điều phối viên tìm trạm sạc gần nhất. 4. Soạn tin nhắn hướng dẫn tài xế. 5. Nếu pin quá thấp thì gọi đội cứu hộ. |
| Bước tốn thời gian/lỗi nhất | Bước 3 và 4, mất khoảng 10-15 phút/lượt vì phải tra cứu và viết hướng dẫn thủ công. |
| AI hỗ trợ ở bước nào? | AI hỗ trợ phân tích tình huống, kiểm tra mức pin, đề xuất hành động an toàn và soạn tin nhắn hướng dẫn dạng nháp. |
| Metric thành công | Giảm thời gian xử lý từ 15 phút xuống dưới 3 phút/lượt; 100% trường hợp pin dưới 5% phải đề xuất cứu hộ thay vì hướng dẫn đi xa. |
| Quick Architecture | LLM Feature kết hợp rule an toàn. |

### Quick Problem Card #2

| Mục | Nội dung |
|---|---|
| Bài toán | Tự động phân loại phản ánh cư dân Vinhomes để chuyển đúng bộ phận xử lý. |
| Công ty thành viên | Vinhomes |
| Ai đang đau? | Cư dân phải chờ phản hồi lâu, nhân viên CSKH phải đọc và phân loại nhiều phản ánh lặp lại mỗi ngày. |
| Workflow hiện tại | 1. Cư dân gửi phản ánh qua app. 2. Nhân viên đọc nội dung. 3. Xác định loại vấn đề. 4. Chuyển ticket cho bộ phận kỹ thuật, an ninh, vệ sinh hoặc ban quản lý. 5. Theo dõi phản hồi. |
| Bước tốn thời gian/lỗi nhất | Bước 2 và 3, mất khoảng 5-8 phút/ticket, dễ chuyển sai nếu nội dung mơ hồ. |
| AI hỗ trợ ở bước nào? | AI đọc nội dung phản ánh, phân loại chủ đề, đánh dấu mức độ khẩn cấp và đề xuất bộ phận xử lý. |
| Metric thành công | 85% ticket được phân loại dưới 10 giây; giảm tỷ lệ chuyển sai bộ phận xuống dưới 5%. |
| Quick Architecture | LLM Feature hoặc Rule + LLM. |

### Quick Problem Card #3

| Mục | Nội dung |
|---|---|
| Bài toán | Hỗ trợ bác sĩ Vinmec soạn tóm tắt hồ sơ xuất viện cho bệnh nhân. |
| Công ty thành viên | Vinmec |
| Ai đang đau? | Bác sĩ mất nhiều thời gian làm giấy tờ, bệnh nhân phải chờ lâu để hoàn tất thủ tục xuất viện. |
| Workflow hiện tại | 1. Bác sĩ xem lại bệnh án. 2. Kiểm tra xét nghiệm và chẩn đoán. 3. Viết tóm tắt quá trình điều trị. 4. Ghi thuốc và hướng dẫn sau xuất viện. 5. In/gửi hồ sơ cho bệnh nhân. |
| Bước tốn thời gian/lỗi nhất | Bước 3 và 4, mất khoảng 20-30 phút/bệnh nhân, dễ thiếu thông tin nếu bác sĩ quá tải. |
| AI hỗ trợ ở bước nào? | AI tạo bản nháp tóm tắt xuất viện từ dữ liệu bệnh án, sau đó bác sĩ kiểm tra và duyệt. |
| Metric thành công | Giảm thời gian soạn hồ sơ từ 30 phút xuống còn 10 phút/bệnh nhân; 100% bản nháp phải được bác sĩ duyệt trước khi gửi. |
| Quick Architecture | LLM Feature với Human-in-the-loop bắt buộc. |

---

## Đề xuất bài toán nên chọn để làm nhóm

Tôi đề xuất chọn **bài toán số 3: Hỗ trợ bác sĩ Vinmec soạn tóm tắt hồ sơ xuất viện cho bệnh nhân** để làm Deep-Dive.

Lý do:

- Bài toán có giá trị thực tế cao vì giúp giảm thời gian làm giấy tờ cho bác sĩ và rút ngắn thời gian chờ của bệnh nhân.
- Workflow hiện tại rõ ràng: bác sĩ xem bệnh án, kiểm tra xét nghiệm/chẩn đoán, viết tóm tắt điều trị, ghi thuốc và hướng dẫn sau xuất viện.
- Có bottleneck rõ: bước viết tóm tắt quá trình điều trị và hướng dẫn sau xuất viện mất khoảng 20-30 phút/bệnh nhân.
- Có metric đo được: giảm thời gian soạn hồ sơ từ 30 phút xuống còn 10 phút/bệnh nhân; 100% bản nháp phải được bác sĩ duyệt trước khi gửi.
- Có operational boundary rất rõ: AI chỉ được tạo bản nháp, không được tự chẩn đoán, không được tự kê đơn, không được gửi hồ sơ cho bệnh nhân nếu chưa có bác sĩ duyệt.
