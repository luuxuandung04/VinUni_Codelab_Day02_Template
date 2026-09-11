# 01-problem-scan.md — Phase 1 SCAN + Phase 2 Quick Card #1 (vukhai248)

## Phase 1 — SCAN (dòng #1 của bạn)

| # | Subsidiary | Lens | Mô tả ngắn bài toán |
|---|------------|------|---------------------|
| 1 | Xanh SM | Stakeholder Pain | Khách hàng Xanh SM không bắt được xe trong các giờ cao điểm hoặc các vị trí đang thiếu xe |
| 2 | Xanh SM | Tốn thời gian | Tài xế báo hết pin giữa đường, điều phối viên tra tay trạm sạc trống + soạn SMS mất 12-15'/lượt (xem 02-deliverable-example.md) |
| 3 | Vinhomes | Lặp lại | Khiếu nại cư dân qua App/tổng đài 1900 2323 89 (nhánh 4) phải route tay tới đúng BQL tòa nhà, quy định phản hồi trong 08 giờ làm việc (nguồn: market.vinhomes.vn) |
| 4 | VinFast | Tốn thời gian | Chủ xe để pin dưới 5% mới tìm trạm (hãng cảnh báo cấm để cạn 0%), tới nơi gặp trụ bận + phí đỗ quá giờ 1.000đ/phút từ phút 31 (nguồn: vinfastauto.com) |
| 5 | Vinmec | Tốn thời gian | Bác sĩ mất 20-30'/bệnh nhân để viết tóm tắt xuất viện từ bệnh án + xét nghiệm (cần verify thực địa, gợi ý từ 03-inspiration-kit.md) |

---

## Phase 2 — QUICK PROBLEM CARD #1 (hoàn chỉnh)

```
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #1                                       │
│                                                             │
│ Bài toán (1 câu): Khách hàng Xanh SM không bắt được xe trong các giờ cao điểm hoặc các vị trí đang thiếu xe  │
│ Công ty thành viên: [ ] VinFast  [x] Xanh SM  [ ] Vinhomes  │
│                     [ ] Vinmec   [ ] Khác (Ghi rõ)________  │
│                                                             │
│ Ai đang đau (Actor)? Khách hàng, doanh nghiệp │
│                                                             │
│ Workflow thủ công hiện tại (3-5 bước):                      │
│   1. Khách hàng đặt xe vào giờ cao điểm hoặc vị trí chưa có xe ──> 2. Hệ thống check lịch sử quá tải + tình trạng giao thông ──> 3. Điều tài xế đến trực trước ở điểm nóng hay quá tải ──> 4. Gán xe gần nhất, khách không phải chờ lâu/hủy                   │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 2-3 điều trước bị bỏ sót, phải xử lý lúc đã tắc (⏱ 10-12 phút/lượt)      │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2-3: dự báo điểm nóng quá tải theo giờ/khu + draft lệnh điều tài xế đến trước 15-20', dispatcher duyệt trước khi gửi. Lúc đường tắc thì điều sau không kịp. │
│                                                             │
│ Đo thành công bằng gì (Metric có số)? Giảm hủy do thiếu xe từ ~25% ──> dưới 12%; chờ trung bình từ 12 min ──> dưới 7 min; tỉ lệ có xe trong 3 phút từ ~60% ──> trên 85% │
│   VD: "Giảm thời gian soạn phản hồi từ 10 min ──> under 2 min"│
│                                                             │
│ Quick Architecture: [ ] No AI  [x] Rule  [x] LLM  [ ] Agent │
└─────────────────────────────────────────────────────────────┘
```

#### 
Lý do: Vào các giừo cao điểm hoặc vị trí cần nhu cầu cao có thể xảy ra tình trạng thiếu xe do lượng người dùng yêu cầu cao. chưa kể vào các giờ cao điểm hay xảy ra tình trạng tắc đường

Ảnh hưởng đến trải nghiệm khách hàng và cả doanh nghiệp khi khách hàng hủy các chuyến xe để gọi các dịch vụ khác

Hệ thống ai cần giải quyết, xác nhận các khu vực nóng để điều phối các tài xế đến trước giúp tăng trải nghiệm dịch vụ

---

## Phase 2 — QUICK PROBLEM CARD #2 (Xanh SM sạc pin — từ bài mẫu)

```
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #2                                       │
│ Bài toán: Tài xế Xanh SM báo hết pin giữa đường cần trạm   │
│ sạc gần nhất hoặc xe cứu hộ.                                │
│ Công ty: [x] Xanh SM                                        │
│ Ai đau? Tài xế (chờ), điều phối viên (quá tải giờ cao điểm) │
│ Workflow: 1. Tài xế gọi báo hết pin ──> 2. Tra GPS xe ──>   │
│ 3. Tra tay trạm trống phù hợp cổng sạc ──> 4. Soạn SMS chỉ  │
│ đường ──> 5. Gọi cứu hộ nếu pin <5%                         │
│ Bottleneck? Bước 3-4 (⏱ 10-12 phút/lượt)                    │
│ AI vào? Bước 3-4: auto lấy GPS + trạm trống + draft SMS,    │
│ dispatcher duyệt (HITL). Pin <5% → đề xuất xe sạc di động,  │
│ cấm chỉ trạm >5km.                                          │
│ Metric? 15 min ──> dưới 3 min; đúng trạm 98%.               │
│ Architecture: [x] LLM Feature                               │
└─────────────────────────────────────────────────────────────┘
```

## Phase 2 — QUICK PROBLEM CARD #3 (Vinhomes route khiếu nại)

```
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #3                                       │
│ Bài toán: Khiếu nại cư dân (mất nước, hỏng đèn, ồn ào) gửi │
│ qua App/tổng đài bị route nhầm BQL, ngâm nhiều giờ.         │
│ Công ty: [x] Vinhomes                                       │
│ Ai đau? Cư dân (chờ), CSKH tổng đài (phân loại tay), BQL    │
│ (nhận việc không thuộc mình)                                │
│ Workflow: 1. Cư dân gửi phản ánh ──> 2. CSKH đọc + đoán     │
│ chủ đề ──> 3. Chuyển tay tới BQL tòa nhà ──> 4. BQL xử lý + │
│ phản hồi trong 08 giờ làm việc ──> 5. Ca khẩn (an toàn,     │
│ tính mạng) phải xử lý ngay                                  │
│ Bottleneck? Bước 2-3 route tay sai ~30% (⏱ hàng giờ/lượt)   │
│ AI vào? Bước 2-3: phân loại + route tự động + draft trả     │
│ lời, nhân viên duyệt. Ca khẩn auto gắn cờ ưu tiên.          │
│ Metric? Route sai 30% ──> dưới 5%; phản hồi 12h ──> dưới 1h │
│ (ca khẩn <15').                                             │
│ Architecture: [x] Rule (từ khóa khẩn) + [x] LLM (phân loại) │
└─────────────────────────────────────────────────────────────┘
```
