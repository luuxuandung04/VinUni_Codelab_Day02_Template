# Báo cáo Nhật ký AI (`03-ai-log.md`)

## 1. Đóng góp cá nhân thực tế
* Xây dựng và kiểm thử bộ test case đối với các từ khóa phủ định trong tiền sử dị ứng (ví dụ: trường hợp mô tả *"Bệnh nhân không dị ứng Penicillin"* nhưng mô hình dễ hiểu nhầm thành *"Dị ứng Penicillin"*).
* Thiết kế cấu trúc JSON Schema cho Structured Output nhằm đảm bảo tính đồng nhất khi tích hợp vào hệ thống EMR của Vinmec.

## 2. AI Error / Hallucination & Rủi ro gặp phải
* **Lỗi Hallucination:** Trong giai đoạn thử nghiệm ban đầu với các ca bệnh mô tả mập mờ, LLM đôi khi tự suy diễn thêm một số tiền sử dị ứng giả định không có trong văn bản gốc (ví dụ: tự thêm cảnh báo dị ứng sulfonamide ngoài văn bản).
* **Rủi ro an toàn:** Nếu không có cơ chế chặn ranh giới (`[DRAFT_ONLY]` và yêu cầu `escalation`), hệ thống có thể gây hiểu lầm cho nhân viên y tế rằng đây là quyết định chẩn đoán chính thức thay vì bản nháp hỗ trợ.

## 3. Cách khắc phục & Tinh chỉnh Prompt / Ranh giới
* **Thắt chặt System Prompt:** Bổ sung ràng buộc tuyệt đối: *"Chỉ trích xuất thông tin xuất hiện tường minh trong văn bản đầu vào; nếu thiếu thông tin hoặc phát hiện xung đột, bắt buộc trả về giá trị cờ rủi ro và yêu cầu escalate."*
* **Bắt buộc tiền tố:** Định dạng mọi kết quả đầu ra luôn bắt đầu bằng thẻ `[DRAFT_ONLY]` để làm rõ ranh giới pháp lý và trách nhiệm chuyên môn.
* **Xác thực tự động:** Kiểm thử qua script Python với các adversarial test cases để đảm bảo hệ thống không bị vượt ranh giới an toàn.
