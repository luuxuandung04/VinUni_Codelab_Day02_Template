# 03 - AI Log & Reflection

## 1. Công cụ AI đã sử dụng

Tôi sử dụng ChatGPT như một trợ lý tư duy để brainstorm bài toán, sắp xếp ý tưởng, viết bản nháp Quick Problem Cards và kiểm tra xem bài toán có phù hợp với yêu cầu của Lab 02 hay không.

---

## 2. AI đã giúp gì

AI giúp tôi ở 4 việc chính:

- Gợi ý các bài toán vận hành trong hệ sinh thái Vingroup, bao gồm Xanh SM, Vinhomes, Vinmec, VinFast và Vinpearl.
- Biến các ý tưởng chung chung thành các bottleneck cụ thể hơn, có actor, workflow, thời gian xử lý và metric thành công.
- Hỗ trợ so sánh 3 bài toán tiềm năng để chọn bài phù hợp nhất cho deep-dive.
- Gợi ý operational boundary cho bài toán y tế, đặc biệt là giới hạn AI chỉ được tạo bản nháp, không được tự chẩn đoán, không được tự kê đơn và phải có bác sĩ duyệt.

---

## 3. Điểm AI trả lời chưa tốt hoặc cần kiểm tra lại

Một số câu trả lời của AI có những con số ước tính như 15 phút/lượt, 5-8 phút/ticket, 20-30 phút/bệnh nhân. Các con số này hợp lý để làm bài lab nhưng không phải dữ liệu chính thức từ doanh nghiệp, vì vậy cần ghi rõ là ước tính.

AI cũng có xu hướng đề xuất dùng LLM cho mọi bài toán. Tuy nhiên, trong lĩnh vực y tế, nếu không có ranh giới rõ ràng thì AI có thể tạo ra rủi ro lớn. Vì vậy tôi phải giới hạn vai trò của AI ở mức tạo bản nháp và nhắc thiếu thông tin, không cho AI ra quyết định chuyên môn.

---

## 4. Tôi đã sửa prompt/ranh giới như thế nào

Ban đầu, prompt chỉ yêu cầu AI gợi ý bài toán AI cho Vingroup. Sau đó tôi sửa prompt theo hướng chặt hơn:

```text
Hãy đóng vai trò CFO và Trưởng phòng Vận hành khó tính. Hãy chỉ ra điểm yếu của bài toán, metric nào chưa rõ, và khi nào rule-based system tốt hơn AI.
```

Prompt này giúp tôi nhìn bài toán thực tế hơn, tránh việc chọn bài quá rộng hoặc quá "AI vì AI".

Với bài toán Vinmec, tôi thêm operational boundary:

- AI chỉ được tạo bản nháp tóm tắt xuất viện.
- AI không được tự chẩn đoán bệnh.
- AI không được tự kê đơn hoặc thay đổi chỉ định điều trị.
- AI không được bịa thông tin nếu dữ liệu bệnh án còn thiếu.
- 100% bản nháp phải được bác sĩ kiểm tra và duyệt trước khi gửi cho bệnh nhân.

---

## 5. Bài học cá nhân

Tôi nhận ra rằng phần khó nhất không phải là nghĩ ra một ý tưởng AI, mà là scope bài toán cho đủ nhỏ, đủ đo được và có ranh giới vận hành rõ ràng. Một ý tưởng AI tốt cần trả lời được: ai đang đau, bước nào đang tắc nghẽn, AI được phép làm gì, AI không được làm gì, và nếu AI sai thì con người sẽ can thiệp ở đâu.

Trong bài này, tôi đề xuất chọn case Vinmec vì bài toán có giá trị thực tế cao, workflow rõ, tác động trực tiếp đến thời gian làm việc của bác sĩ và bắt buộc phải thể hiện tư duy Human-in-the-loop trong lĩnh vực rủi ro cao.
