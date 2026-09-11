AI Prompts — Brainstorm bài toán vận hành: Xanh SM
Mảng đã chọn: Xanh SM (taxi điện & dịch vụ gọi xe)
1. Danh sách 5 pain point vận hành tiềm năng

Lưu ý: các con số dưới đây là ước tính minh họa dựa trên benchmark ngành ride-hailing/xe điện nói chung, không phải số liệu nội bộ đã xác nhận của Vingroup. Cần đối chiếu với dữ liệu vận hành thực tế trước khi đưa vào proposal.

1.1. Điều phối xe & dự báo nhu cầu theo khung giờ/khu vực
Vấn đề: Phân bổ tài xế dựa trên kinh nghiệm/lịch sử thủ công, chưa tối ưu theo real-time demand.
Ước tính tổn thất: Ngành ride-hailing thường mất 15-25% thời gian xe chạy rỗng (deadheading) do điều phối chưa tối ưu → tương đương giảm 10-15% doanh thu/xe/ngày.
1.2. Lập lịch sạc pin & quản lý trạm sạc
Vấn đề: Tài xế tự quyết định thời điểm sạc, không đồng bộ với nhu cầu cao điểm, gây tắc nghẽn trạm sạc hoặc xe hết pin giữa chuyến.
Ước tính tổn thất: Downtime sạc không tối ưu có thể chiếm 1-2 giờ/xe/ngày lẽ ra dùng để chở khách → mất khoảng 8-12% năng suất khai thác xe.
1.3. Bảo trì dự đoán (predictive maintenance) cho xe điện
Vấn đề: Bảo trì theo lịch cố định (km/thời gian) thay vì theo tình trạng pin/động cơ thực tế, dẫn tới hỏng hóc đột xuất hoặc bảo trì thừa.
Ước tính tổn thất: Chi phí bảo trì reactive thường cao hơn 30-40% so với predictive; mỗi lần xe phải dừng đột xuất mất trung bình nửa ngày khai thác.
1.4. Xử lý khiếu nại & tranh chấp đánh giá (rating/complaint handling)
Vấn đề: Nhân viên CSKH đọc và phân loại khiếu nại thủ công, xác minh qua GPS/camera hành trình mất nhiều thời gian.
Ước tính tổn thất: Thời gian xử lý trung bình 1 ticket khiếu nại thủ công 20-30 phút; với hàng nghìn ticket/tháng, tương đương hàng trăm giờ nhân sự CSKH mỗi tháng.
1.5. Đối soát doanh thu & phát hiện gian lận cuốc xe
Vấn đề: Đối chiếu doanh thu giữa app, ví điện tử, tiền mặt và phát hiện các cuốc xe bất thường (hủy ảo, quay vòng GPS) làm thủ công hoặc bằng rule cứng.
Ước tính tổn thất: Gian lận cuốc xe trong ngành ride-hailing thường chiếm 1-3% tổng doanh thu nếu không có hệ thống phát hiện tự động hiệu quả.
2. Use case đào sâu: Điều phối xe & dự báo nhu cầu
2.1. Đánh giá khắt khe (vai CFO / Trưởng phòng Vận hành)

a. Điểm yếu về logic

Bài toán giả định rằng dự báo tốt hơn → điều phối tốt hơn → doanh thu tăng. Chuỗi nhân quả này có ít nhất 2 lỗ hổng:

Tài xế không phải robot. Dù mô hình AI dự báo chính xác "khu A sẽ có nhu cầu cao lúc 17h", không có cơ chế ép tài xế di chuyển đến đó. Nếu không giải được bài toán incentive trước, model dự báo giỏi đến đâu cũng vô nghĩa.
Feedback loop tự phá hoại. Nếu AI liên tục điều xe đến khu A vì "dự báo cao", cung sẽ tăng ở đó → nhu cầu thực tế bị pha loãng → dự báo cho kỳ sau lại sai theo hướng ngược lại (self-defeating prediction). ML thường không tự nhận ra vòng lặp này trừ khi được thiết kế như hệ thống closed-loop (gần với reinforcement learning, phức tạp và rủi ro hơn nhiều so với dự báo đơn thuần).

b. Điểm yếu về metric

Metric kiểu "độ chính xác dự báo nhu cầu" (MAPE, RMSE...) là vanity metric trong ngữ cảnh này:

MAPE thấp không đảm bảo tỷ lệ chuyển đổi gợi ý → tài xế di chuyển thật cũng cao. Đây mới là driver ra tiền.
Metric đúng cần đo: doanh thu/xe/giờ tăng bao nhiêu % sau khi triển khai, so với baseline không có AI, kèm A/B test plan.
Thiếu hẳn metric về chi phí vận hành hệ thống (compute, latency, chi phí maintain pipeline dữ liệu real-time) để tính ROI.

c. Vì sao rule-based có thể thắng AI ở giai đoạn này

Nhu cầu ride-hailing có tính chu kỳ rất mạnh và ổn định (giờ cao điểm, khu vực văn phòng vs dân cư, cuối tuần vs ngày thường). Heuristic dựa trên trung bình trượt theo khung giờ + ngày trong tuần (lịch sử 4-8 tuần gần nhất) có thể nắm bắt 70-80% pattern mà không cần ML.
Rule-based có ưu thế: giải thích được, debug được trong 5 phút, triển khai/bảo trì rẻ — không cần đội data scientist retrain model, không cần lo model drift.
AI/ML chỉ đáng đầu tư khi cần bắt pattern phi tuyến, nhiều biến tương tác mà rule không nắm được (thời tiết, sự kiện đột xuất, dữ liệu real-time đa nguồn). Nếu chưa chứng minh rule-based đã chạm trần hiệu quả, đầu tư AI ngay là giải pháp thừa cân so với vấn đề.

Yêu cầu trước khi duyệt ngân sách:

Chạy baseline rule-based (moving average + calendar rules) trước, đo hiệu quả thực tế.
Chỉ ra khoảng cách hiệu quả (gap) mà rule-based không giải được — đó mới là phạm vi AI nên nhắm vào.
Thiết kế lại metric theo doanh thu/xe, không phải độ chính xác thống kê.
2.2. Quy trình thủ công hiện tại (6 bước)
#	Bước	Loại	Thời gian
1	Thu thập dữ liệu cuối ca	Bình thường	5 phút
2	Chuyển dữ liệu cho điều phối viên	🔄 Handoff (hệ thống → người)	10 phút
3	Ước lượng nhu cầu (thủ công)	🔴 Bottleneck (dựa kinh nghiệm, chủ quan)	15 phút
4	Thông báo tài xế (gọi điện/nhắn tin)	🔄 Handoff (người → tài xế)	10 phút
5	Tài xế di chuyển khu vực	🔴 Bottleneck (không xác nhận real-time)	20 phút
6	Ghi nhận kết quả cuối ca	Bình thường	10 phút

Tổng cộng = 70 phút/lượt

Chi tiết bước 6 — Ghi nhận kết quả cuối ca:

Nội dung: số cuốc hoàn thành theo khu vực/khung giờ, số lần đề xuất điều phối bị bỏ qua, thời gian chạy rỗng thực tế, ghi chú bất thường.
Không phải bottleneck về thao tác, nhưng là nguồn dữ liệu đầu vào cho chu kỳ tiếp theo — ghi nhận thiếu chuẩn hóa sẽ làm nhiễu toàn bộ vòng lặp dự báo phía sau ("garbage in, garbage out").
Đây cũng là điểm duy nhất đo được hiệu quả thực tế của gợi ý điều phối (tài xế có nghe theo hay không) — dữ liệu cần thiết để tính ROI.
Gợi ý cải tiến: tự động log dữ liệu ngay khi cuốc xe hoàn thành qua app tài xế, biến bước này thành byproduct tự động thay vì thao tác thủ công.
3. Quick Problem Card #01
QUICK PROBLEM CARD #01

Bài toán (1 câu): Điều phối xe theo khung giờ/khu vực đang dựa vào
kinh nghiệm thủ công của điều phối viên, gây xe chạy rỗng và thất thoát doanh thu.

Công ty thành viên: [ ] VinFast   [x] Xanh SM   [ ] Vinhomes
                     [ ] Vinmec   [ ] Khác (Ghi rõ)________

Ai đang đau (Actor)? Điều phối viên vận hành & tài xế Xanh SM

Workflow thủ công hiện tại (3-5 bước):
  1. Thu thập dữ liệu cuối ca --> 2. Điều phối viên ước lượng nhu cầu
  --> 3. Thông báo tài xế (gọi/nhắn tin) --> 4. Tài xế di chuyển khu vực

Bước nào tốn thời gian/lỗi nhất? Bước 2 - Ước lượng nhu cầu thủ công
(⏱ 15 phút/lượt, sai lệch do chủ quan cá nhân)
AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2 - Dự báo nhu cầu theo
khung giờ/khu vực + gợi ý phân bổ xe tự động

Đo thành công bằng gì (Metric có số)? Giảm tỷ lệ xe chạy rỗng
(deadheading) từ 20% xuống dưới 12%; tăng doanh thu/xe/giờ thêm 8-10%
   VD: "Giảm thời gian ước lượng nhu cầu từ 15 phút --> dưới 3 phút/lượt"

Quick Architecture: [ ] No AI   [x] Rule   [ ] LLM   [ ] Agent

Ghi chú:

Metric chọn "tỷ lệ xe chạy rỗng" và "doanh thu/xe/giờ" thay vì "độ chính xác dự báo" — theo đúng góc nhìn CFO đã phân tích ở phần 2.1.
Architecture chọn Rule, không phải LLM/Agent — vì bài toán dự báo theo chu kỳ giờ/ngày phù hợp với heuristic chi phí thấp. Chỉ nâng cấp lên ML sau khi đã benchmark rule-based và chỉ ra gap rõ ràng.
Các con số (15 phút, 20%, 12%...) là ước lượng minh họa, cần thay bằng số liệu vận hành thật của Xanh SM trước khi đưa vào bản trình bày chính thức.
4. Bước tiếp theo đề xuất
 Đối chiếu số liệu ước tính với dữ liệu vận hành thực tế
 Chạy baseline rule-based (moving average + calendar rules) để làm điểm so sánh
 Vẽ sơ đồ quy trình sau khi có AI để so sánh song song (before/after)
 Thiết kế A/B test plan đo doanh thu/xe/giờ
 Thu hẹp phạm vi AI vào phần rule-based không giải quyết được (pattern phi tuyến: thời tiết, sự kiện đột xuất...)