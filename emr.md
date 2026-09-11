# ĐỀ ÁN AI LAB: HỆ THỐNG KIỂM TRA & SỬA LỖI HỒ SƠ BỆNH ÁN TỰ ĐỘNG (EMR AUDIT ASSISTANT)
**Chương trình:** AICB-P1: AI Practical Competency Program, Phase 1  
**Đơn vị/Bối cảnh:** Khoa Khám bệnh & Phòng Quản lý Chất lượng — Bệnh viện / Khối Y tế Số  
**Nhóm thực hiện:** Nhóm KHDL1 — Khoa học Dữ liệu Y tế  

---

## 🏛️ Bối cảnh: Tôi là ai?


Thông qua khảo sát thực tế tại các phòng khám ngoại trú, chúng tôi nhận thấy các bác sĩ và nhân sự y tế đang phải chịu áp lực ghi chép hồ sơ bệnh án điện tử (EMR) khổng lồ. Việc này dẫn đến tình trạng thường xuyên xảy ra lỗi đánh máy (`typo`), bỏ sót trường thông tin bắt buộc (`missing field`) hoặc vi phạm quy chuẩn tuân thủ bảo hiểm y tế (`compliance issue`), làm gia tăng thời gian chờ đợi của người bệnh và rủi ro xuất toán bảo hiểm. Bài toán chúng tôi mang đến buổi Lab hôm nay xuất phát từ chính nỗi đau vận hành cấp bách này.

---

# 🔍 Phase 1 — SCAN: Tìm kiếm cơ hội (Cá nhân)

Sử dụng **4 Lenses** quét qua toàn diện hoạt động vận hành khối y tế và bệnh viện:

| # | Đơn vị / Khoa | Lens | Mô tả ngắn bài toán |
|---|---------------|------|---------------------|
| 1 | **Khoa Khám bệnh** | Tốn thời gian | Bác sĩ mất 5-7 phút sau mỗi ca khám để dò lỗi chính tả, chỉnh sửa thuật ngữ y khoa viết tắt thủ công trên EMR. |
| 2 | **Phòng BHYT** | Lặp lại | Hồ sơ bệnh án bị từ chối thanh toán (xuất toán BHYT) do thiếu mã ICD-10 hoặc sai lệch thông tin chỉ định lâm sàng. |
| 3 | **Khoa Dược** | Lặp lại | Đối chiếu và phát hiện tương tác thuốc hoặc sai liều lượng kê đơn trên hệ thống ngoại trú. |
| 4 | **Khoa Cấp cứu** | AI-upgrade | Hệ thống tự động phân loại mức độ ưu tiên (`Triage`) dựa trên triệu chứng nhập viện ban đầu của bệnh nhân. |
| 5 | **Khoa Xét nghiệm** | Tốn thời gian | Nhập liệu thủ công kết quả từ các máy xét nghiệm đời cũ không tích hợp LIS vào phần mềm trung tâm. |
| 6 | **Phòng Kế hoạch tổng hợp**| Tốn thời gian | Tóm tắt lịch sử bệnh án dài dòng thành báo cáo ngắn gọn phục vụ hội chẩn liên viện. |

---

# 🃏 Phase 2 — QUICK-ASSESS: 3 Quick Problem Cards (Cá nhân)

Chọn top 3 từ danh sách SCAN để đánh giá nhanh: **#1 (Khoa Khám bệnh Typo/Missing), #2 (Phòng BHYT Compliance), #3 (Khoa Dược Tương tác thuốc).**

## Thẻ bài toán tiêu biểu được chọn Deep-Dive: Card #1 — Kiểm tra & Sửa lỗi hồ sơ bệnh án tự động

```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #1                                       │
│                                                             │
│ Bài toán: Bác sĩ nhập liệu hồ sơ bệnh án điện tử (EMR)      │
│ thường xuyên mắc lỗi đánh máy (typo), thiếu trường dữ liệu  │
│ bắt buộc (missing field) và sai quy chuẩn BHYT (compliance).│
│ Đơn vị: [x] Khoa Khám bệnh & QLCL - Bệnh viện               │
│                                                             │
│ Ai đang đau? Bác sĩ (quá tải), Nhân viên QLCL (rà soát mệt) │
│                                                             │
│ Workflow thủ công hiện tại (5 bước):                        │
│   1. Bác sĩ hoàn tất khám và gõ nhanh nội dung bệnh án      │
│   → 2. Lưu và gửi hồ sơ lên hệ thống EMR chung              │
│   → 3. Nhân viên Phòng QLCL tiến hành lấy mẫu kiểm tra ngẫu nhiên│
│   → 4. Phát hiện lỗi chính tả, thiếu trường hoặc sai mã BHYT│
│   → 5. Gửi email yêu cầu bác sĩ mở lại hồ sơ để chỉnh sửa   │
│                                                             │
│ Bước nào tốn nhất? Bước 3-5 (⏱ 20-30 phút/hồ sơ bị lỗi)     │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Ngay tại Bước 1-2     │
│ (Tự động quét thời gian thực -> Sửa typo -> Cảnh báo thiếu) │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                        │
│ Giảm tỷ lệ hồ sơ lỗi từ 12% xuống dưới 1% trước khi lưu.   │
│                                                             │
│ Quick Architecture: [x] LLM Feature (Xử lý ngôn ngữ y tế)   │
└─────────────────────────────────────────────────────────────┘