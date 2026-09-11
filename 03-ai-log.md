# 03-ai-log.md — Nhật ký tương tác AI (vukhai248)

> Phản ánh quá trình sử dụng AI làm trợ lý đồng hành trong Lab 02: nêu rõ AI giúp gì, sai/hallucination ở đâu, và đã sửa prompt/ranh giới ra sao.

---

## 1. AI đã giúp gì

### Giai đoạn Phase 1 — SCAN

Khi bắt đầu liệt kê bài toán cho 5 công ty thành viên Vingroup, AI được dùng như một "partner brainstorm" — đưa vào danh sách công ty và yêu cầu gợi ý các luồng vận hành thủ công có khả năng tối ưu bằng AI. Kết quả hữu ích: AI gợi ý nhanh pattern lặp lại nhiều trong ngành vận tải điện (điều phối tài xế, quản lý sạc) và bất động sản (phân loại khiếu nại cư dân), giúp định hướng chọn được 5 bài toán đủ đa dạng cho bảng SCAN trong `01-problem-scan.md`.

### Giai đoạn Phase 2 — QUICK PROBLEM CARDS

AI giúp cấu trúc hóa 3 Quick Problem Cards theo đúng khung yêu cầu: actor, workflow 3–5 bước, bottleneck có số thời gian, điểm AI có thể can thiệp, metric có số, và kiến trúc (Rule/LLM/Agent). Cụ thể với Card #2 (Xanh SM — sạc pin), AI đã giúp phân tách rõ 5 bước tuần tự và xác định bước 3–4 là bottleneck chính (tra tay trạm trống + soạn SMS mất 10–12 phút/lượt).

### Giai đoạn Phase 3/4 — Workflow & Prompt Boundary

AI hỗ trợ đề xuất kiến trúc giải pháp `Rule + LLM Feature` cho bài toán điều phối xe hotspot, và giải thích lý do không cần dùng Agent tự trị (quy trình cố định, bắt buộc HITL). AI cũng đề xuất các ràng buộc an toàn cho `SYSTEM_PROMPT`: output phải gắn nhãn `[DRAFT_ONLY]`, pin < 5% phải `dispatch_mobile_charger`, không đề xuất trạm > 5km.

---

## 2. AI sai / Hallucination phát sinh

### Lỗi 1 — Bịa số liệu không có nguồn

**Vấn đề:** Khi hỏi số lượng trạm sạc VinFast tại Hà Nội, AI trả về một con số cụ thể nghe rất hợp lý. Không có trích dẫn URL, không thể xác minh. Nếu dùng ngay vào báo cáo như một fact sẽ là số liệu bịa (unfounded claim).

**Cách sửa prompt:** Thêm ràng buộc vào câu hỏi: *"Chỉ trả lời nếu có thể trích dẫn nguồn cụ thể. Nếu không có nguồn, trả lời 'cần verify thực địa' thay vì đưa số."* Kết quả: các số liệu không có nguồn đều bị loại khỏi bảng SCAN, thay bằng ghi chú `(cần verify thực địa)`. Chỉ giữ lại số liệu lấy từ nguồn chính thức có thể kiểm tra (vd: phí đỗ quá giờ 1.000đ/phút từ phút 31 — nguồn vinfastauto.com).

---

### Lỗi 2 — Metric quá lạc quan, không có cơ sở thực tế

**Vấn đề:** Khi yêu cầu đề xuất metric thành công cho bài toán điều phối xe Xanh SM, AI đưa ra con số cải thiện rất lớn (ví dụ giảm thời gian chờ từ 15 phút xuống dưới 2 phút). Con số này không thực tế với giải pháp dự báo + điều phối trước vì còn phụ thuộc vào giao thông thực địa và thời gian dispatcher duyệt.

**Cách sửa prompt:** Thêm ngữ cảnh: *"Đề xuất metric thực tế, có so sánh với benchmark ngành ride-hailing tương tự. Gắn nhãn 'ước tính' nếu không có dữ liệu thực địa."* Sau khi điều chỉnh, metric trong Quick Problem Card #1 được cập nhật thực tế hơn: thời gian chờ từ 12 phút → dưới 7 phút, tỉ lệ hủy từ ~25% → dưới 12% — phù hợp với bối cảnh điều phối trước 15–20 phút.

---

### Lỗi 3 — Hallucination về chính sách nội bộ công ty

**Vấn đề:** Khi phân tích bài toán Vinhomes route khiếu nại (Card #3), AI tự điền thời gian phản hồi trung bình cao hơn nhiều so với quy định thực tế. Tuy nhiên, thông tin trên market.vinhomes.vn ghi rõ quy định phản hồi trong 08 giờ làm việc — khác hoàn toàn với con số AI đưa ra.

**Cách sửa ranh giới:** Cung cấp URL nguồn trực tiếp vào câu hỏi và thêm ràng buộc: *"Ưu tiên thông tin từ URL đã cung cấp. Không tự điền số liệu về chính sách nội bộ doanh nghiệp khi không có tài liệu được cấp."* Sau khi sửa, Quick Problem Card #3 dùng đúng số liệu từ nguồn: phản hồi trong quy định 08 giờ làm việc, với mục tiêu AI giúp rút xuống dưới 1 giờ.

---

## 3. Tổng kết — Ranh giới sử dụng AI trong bài

| Dùng AI được | Không để AI tự quyết |
|---|---|
| Brainstorm bài toán, gợi ý workflow | Cung cấp số liệu thực địa làm fact |
| Cấu trúc hóa nội dung, soạn template | Xác nhận chính sách nội bộ doanh nghiệp |
| Kiểm tra logic prompt, đề xuất kiến trúc | Đưa ra metric cuối không có nguồn cơ sở |
| Draft SYSTEM_PROMPT và test boundary | Thay thế phỏng vấn / quan sát thực địa |

> Nguyên tắc xuyên suốt: mọi số liệu do AI sinh ra đều được gắn nhãn *(ước tính — cần verify)* cho đến khi có nguồn chính thức hoặc dữ liệu thực địa xác nhận. Ranh giới này áp dụng nhất quán từ bảng SCAN (01) đến workflow diagram (04) và prompt prototype.

---

*Cập nhật lần cuối: 2026-09-11 | Người thực hiện: vukhai248*
