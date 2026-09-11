# 03-ai-log.md — AI Interaction Log (vukhai248)

> Ghi lại cách AI được sử dụng trong quá trình nghiên cứu và xây dựng giải pháp, bao gồm những điểm hữu ích, lỗi/hallucination phát sinh và cách khắc phục.

---

## 1. AI đã hỗ trợ những gì

### 1.1 Research bài toán Xanh SM — điều phối xe & trạm sạc

**Công cụ sử dụng:** Gemini 3.1 Pro, ChatGPT

**Việc AI giúp được:**

- **Brainstorming các bài toán Vingroup:** Khi đưa danh sách các công ty thành viên (Xanh SM, VinFast, Vinhomes, Vinmec), AI gợi ý nhanh các luồng workflow thủ công có thể cải thiện bằng AI — đặc biệt là bài toán điều phối tài xế và quản lý trạm sạc.
- **Xây dựng workflow 3–5 bước:** AI giúp cấu trúc lại workflow thủ công của điều phối viên Xanh SM thành dạng bước tuần tự rõ ràng (gọi báo hết pin → tra GPS → tra trạm trống → soạn SMS → gọi cứu hộ), từ đó dễ xác định bottleneck ở bước 3–4.
- **Gợi ý kiến trúc giải pháp:** AI gợi ý pattern LLM + HITL (Human-in-the-loop) phù hợp với bài toán dispatcher cần duyệt trước khi gửi thông tin cho tài xế — tránh rủi ro AI tự gửi thông tin sai.
- **Soạn thảo SYSTEM_PROMPT cho prompt_prototype.py:** AI đề xuất cấu trúc prompt có ranh giới an toàn (output [DRAFT_ONLY], giới hạn trạm < 5 km, bắt buộc escalate khi pin < 5%).
- **Phân tích bài toán Vinhomes route khiếu nại:** AI giúp ước lượng tỉ lệ route sai (~30%) và thời gian phản hồi (~12 giờ) dựa trên pattern tổng đài CSKH bất động sản điển hình — cần gắn nhãn "ước tính" vì chưa có dữ liệu thực địa Vinhomes.

---

### 1.2 Hỗ trợ xây dựng Quick Problem Cards

AI giúp kiểm tra tính nhất quán của 3 Quick Problem Cards: đảm bảo mỗi card có đủ actor, workflow 3–5 bước, bottleneck có số thời gian, AI solution gắn đúng bước, metric có số và kiến trúc được gắn nhãn (Rule/LLM/Agent).

---

## 2. Lỗi / Hallucination phát sinh

### Lỗi 1: Số liệu về trạm sạc VinFast bịa đặt

**Mô tả:** Khi hỏi "Có bao nhiêu trạm sạc VinFast tại Hà Nội?", AI (ChatGPT-4o) trả lời cụ thể "hơn 150 trạm sạc tại 80+ địa điểm ở Hà Nội". Số liệu này nghe có vẻ hợp lý nhưng không trích dẫn nguồn và không thể xác minh qua vinfastauto.com hay vinfast.vn.

**Rủi ro:** Nếu đưa con số này vào báo cáo như một fact, sẽ bị đánh giá là số liệu bịa (unfounded claim).

**Cách khắc phục:**
- Sửa prompt: "Chỉ trả lời nếu có thể trích dẫn URL nguồn cụ thể. Nếu không chắc, hãy nói 'cần verify thực địa' thay vì đưa con số."
- Trong 01-problem-scan.md, các số liệu chưa có nguồn được gắn chú thích (cần verify thực địa) hoặc chỉ dùng nguồn chính thức đã kiểm tra (vinfastauto.com — phí đỗ quá giờ 1.000đ/phút từ phút 31).

---

### Lỗi 2: AI đề xuất metric quá lạc quan, thiếu cơ sở

**Mô tả:** Khi được yêu cầu đề xuất metric thành công cho bài toán điều phối xe Xanh SM, AI đưa ra "giảm thời gian chờ từ 15 phút xuống dưới 2 phút" — một con số không thực tế với giải pháp dự báo + điều phối trước.

**Rủi ro:** Metric quá lạc quan làm giảm độ tin cậy của toàn bộ phân tích. Reviewer/giảng viên có thể thấy đây là "ước lượng AI không có cơ sở thực tế".

**Cách khắc phục:**
- Sửa prompt: "Đề xuất metric thực tế, so sánh với benchmark ngành ride-hailing (Grab, Be tại Việt Nam). Gắn nhãn rõ 'ước tính' nếu không có dữ liệu thực địa Xanh SM."
- Điều chỉnh metric trong Quick Problem Card #1: thời gian chờ từ 12 min → dưới 7 min (hợp lý hơn cho bối cảnh điều phối trước 15–20').
- Quick Problem Card #2 (trạm sạc): 15 min → dưới 3 min — giữ nguyên vì bước soạn SMS và tra tay chiếm phần lớn thời gian, hoàn toàn tự động hóa được.

---

### Lỗi 3: Hallucination về thời gian phản hồi Vinhomes

**Mô tả:** AI tự điền "thời gian phản hồi trung bình 24–48 giờ" cho khiếu nại Vinhomes mà không có nguồn. Thực tế, thông tin trên market.vinhomes.vn chỉ nêu "quy định 08 giờ làm việc" — không phải 24–48 giờ.

**Cách khắc phục:**
- Sửa prompt với ranh giới rõ: "Ưu tiên dùng thông tin từ URL đã cung cấp. Không tự điền số liệu ngoài phạm vi tài liệu được cấp."
- Cập nhật Quick Problem Card #3: dùng "phản hồi 12h → dưới 1h" thay vì con số AI bịa.

---

## 3. Tổng kết — Ranh giới sử dụng AI

| Phù hợp để dùng AI | Không nên để AI tự quyết |
|---|---|
| Brainstorm bài toán, gợi ý workflow | Cung cấp số liệu cụ thể làm fact |
| Cấu trúc hóa nội dung, draft template | Xác nhận số liệu thực địa doanh nghiệp |
| Soạn thảo prompt, kiểm tra logic | Đưa ra metric cuối cùng không có nguồn |
| Đề xuất kiến trúc giải pháp (Rule/LLM/Agent) | Thay thế research thực địa / phỏng vấn stakeholder |

> **Nguyên tắc áp dụng xuyên suốt:** Mọi số liệu do AI sinh ra đều được gắn nhãn (ước tính — cần verify) cho đến khi tìm được nguồn chính thức hoặc dữ liệu thực địa xác nhận.

---

*Cập nhật lần cuối: 2026-09-11 | Người thực hiện: vukhai248*
