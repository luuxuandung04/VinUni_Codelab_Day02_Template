# Báo cáo cá nhân: 5 Bài toán & Quick Problem Cards (Hệ thống Y tế Vinmec / Vingroup)

## 1. Danh sách 5 bài toán tại hệ thống y tế Vinmec (Vingroup)
1. **Trích xuất tự động báo cáo sự cố y khoa (Adverse Event Reports):** Chuyển đổi các ghi chú tường thuật phi cấu trúc của điều dưỡng thành dữ liệu có cấu trúc để phân tích nguyên nhân lỗi thực hiện quy trình.
2. **Phát hiện sót dị ứng thuốc (Allergy Miss) trước y lệnh:** Quét lịch sử tiền sử dị ứng và đối chiếu với đơn thuốc mới trong EMR (Electronic Medical Record) nhằm ngăn ngừa sai sót cấp phát.
3. **Phân loại mức độ tổn hại sự cố y khoa tự động:** Tự động gán nhãn phân độ tổn hại theo tiêu chuẩn quốc tế (NCC MERP / WHO) dựa trên mô tả diễn biến lâm sàng.
4. **Tổng hợp tự động biên bản RCA (Root Cause Analysis):** Tóm tắt các yếu tố góp phần gây ra sự cố từ biên bản họp hội đồng khoa học kỹ thuật bệnh viện.
5. **Giám sát sai sót dùng thuốc ngoại trú (Outpatient Medication Error Screening):** Phân tích phản hồi từ người bệnh qua cổng thông tin Vinmec để phát hiện các trường hợp uống sai liều hoặc nhầm thuốc.

---

## 2. 3 Quick Problem Cards

### Card 1: Phát hiện sót dị ứng thuốc trong y lệnh (Allergy Miss Detection)
* **Actor:** Dược sĩ lâm sàng / Bác sĩ điều trị.
* **Workflow (4 bước):** 
  1. Trích xuất văn bản từ phiếu y lệnh và tiền sử dị ứng.
  2. Mô hình phân tích thực thể y tế (NER) nhận diện hoạt chất và chất gây dị ứng.
  3. So khớp chéo danh mục dược lý (Drug-Allergy Cross-checking).
  4. Cảnh báo nếu phát hiện xung đột hoặc thiếu sót.
* **Bottleneck / Thời gian:** Dược sĩ mất trung bình 4–6 phút kiểm tra thủ công từng hồ sơ bệnh nhân phức tạp; nguy cơ sót do quá tải công việc.
* **AI Solution:** Mô hình LLM kết hợp quy tắc từ điển (Rule-based + LLM) quét toàn bộ hồ sơ trong vòng 1.2 giây, đánh dấu các cặp xung đột tiềm ẩn.
* **Metric có số:** Độ chính xác phân loại dị ứng $\ge 96.5\%$; giảm thời gian kiểm tra y lệnh xuống dưới 30 giây/ca.
* **Kiến trúc:** Hybrid (Rule-based regex cho danh mục dị ứng + LLM trích xuất ngữ cảnh + Agent xác thực chéo).

### Card 2: Trích xuất lỗi thực hiện quy trình từ báo cáo sự cố
* **Actor:** Nhân viên quản lý chất lượng bệnh viện (Patient Safety Officer).
* **Workflow (4 bước):** 
  1. Tiếp nhận báo cáo sự cố dạng văn bản tự do.
  2. LLM trích xuất các mốc thời gian, nhân sự liên quan, và bước quy trình bị lỗi (ví dụ: sai liều, nhầm bệnh nhân, chậm thời gian).
  3. Chuẩn hóa theo danh mục sự cố quốc gia.
  4. Lưu trữ vào cơ sở dữ liệu phân tích xu hướng.
* **Bottleneck / Thời gian:** Xử lý và phân loại thủ công mất khoảng 15–20 phút mỗi báo cáo; khó tổng hợp nhanh báo cáo tháng/quý.
* **AI Solution:** Prompt có cấu trúc nghiêm ngặt trích xuất JSON các trường thông tin chuẩn hóa sự cố y khoa.
* **Metric có số:** Giảm 75% thời gian nhập liệu thủ công; độ phủ trường thông tin trích xuất đạt $\ge 90\%$.
* **Kiến trúc:** LLM (Structured Output JSON).

### Card 3: Phân loại mức độ nghiêm trọng sự cố y khoa (Severity Triage)
* **Actor:** Hội đồng quản lý rủi ro lâm sàng.
* **Workflow (3 bước):** 
  1. Đọc mô tả hậu quả lâm sàng của sự cố.
  2. Đánh giá mức độ tổn hại từ Cấp độ A đến I theo khung chuẩn.
  3. Đề xuất mức độ ưu tiên điều tra nguyên nhân.
* **Bottleneck / Thời gian:** Sự bất đồng quan điểm giữa các thành viên hội đồng khi phân loại sơ bộ các ca ranh giới (borderline cases), kéo dài thời gian họp hội đồng từ 1–2 ngày.
* **AI Solution:** Agent chấm điểm rủi ro dựa trên tiêu chí định sẵn, đưa ra phác thảo phân loại kèm lý giải bằng y văn/quy định nội bộ.
* **Metric có số:** Độ đồng thuận sơ bộ với hội đồng chuyên môn $\ge 88\%$; rút ngắn chu kỳ phản hồi sự cố cấp tính xuống dưới 2 giờ.
* **Kiến trúc:** Agent (RAG với quy trình quản lý sự cố Vinmec + LLM Reasoning).
