# 04-workflow-diagram.md — Workflow Diagram (vukhai248)

> **Use Case:** Xanh SM — Điều phối xe giờ cao điểm (Hotspot Pre-positioning)
> **Bài toán:** Khách hàng không bắt được xe vào giờ cao điểm do hệ thống điều phối xe phản ứng muộn, thiếu xe ở điểm nóng.

---

## 3.1 Current-State Workflow

Quy trình hiện tại khi khách đặt xe vào giờ cao điểm, hệ thống điều phối phản ứng bị động:

```text
┌──────────────┐     ┌──────────────┐     ┌──────────────┐     ┌──────────────┐
│ Bước 1       │     │ Bước 2       │     │ Bước 3       │     │ Bước 4       │
│ Khách đặt xe │     │ Hệ thống tìm │     │ Không có xe  │     │ Tài xế xa từ │
│ qua app      │ ──→ │ xe trong BK  │ ──→ │ → báo chờ /  │ ──→ │ chối / kẹt   │
│              │     │ 2km          │     │ nới bán kính │     │ xe không tới │
│ Ai: Khách    │     │ Ai: Hệ thống │     │ Ai: Hệ thống │     │ Ai: Tài xế   │
│ ⏱ 1 phút     │     │ ⏱ 2 phút     │     │ ⏱ 5 phút 🔴  │     │ ⏱ 4 phút 🔴  │
│ In: Yêu cầu  │     │ In: Vị trí   │     │ In: Không có │     │ In: Điều     │
│ Out: Đặt xe  │     │ Out: Danh    │     │ xe phù hợp   │     │ phối tài xế  │
│              │     │ sách tài xế  │     │ Out: Thông   │     │ Out: Từ chối │
└──────────────┘     └──────────────┘     │ báo chờ      │     │ / delay      │
                                          └──────────────┘     └──────────────┘
                                                                      │
                                                                      ▼
                                                             ┌──────────────┐
                                                             │ Bước 5       │
                                                             │ Khách chờ    │
                                                             │ quá lâu →    │
                                                             │ hủy chuyến   │
                                                             │ Ai: Khách    │
                                                             │ ⏱ biến thiên │
                                                             └──────────────┘
🔴 = Bottlenecks
⏱ Tổng thời gian thủ công: ~12 phút/lượt (giờ cao điểm)
```

---

## 3.2 Problem Statement (6-field) — Vin Smart Future Standard

| Field | Nội dung |
|---|---|
| **1. Actor / Operator** | Điều phối viên (Dispatcher) và hệ thống điều phối xe của Xanh SM. |
| **2. Current Workflow** | Khi khách đặt xe, hệ thống tìm xe trong bán kính 2km. Nếu không có xe, báo chờ hoặc nới bán kính. Tài xế ở xa có thể từ chối hoặc kẹt đường không đến kịp. Khách chờ >10 phút thường hủy chuyến. Toàn bộ điều phối phản ứng bị động theo yêu cầu, không có dự báo trước. |
| **3. Bottleneck** | Bước 3–4 (⏱ 9 phút): Không có xe sẵn sàng ở điểm nóng trước giờ cao điểm; điều phối sau khi tắc không kịp, dẫn đến hủy chuyến ~25%. |
| **4. Business Impact** | Tỉ lệ hủy do thiếu xe ~25% giờ cao điểm. Mỗi chuyến hủy mất doanh thu trực tiếp + tổn hại trải nghiệm khách hàng. Tài xế thiếu cuốc khi di chuyển đến vùng vắng, gây lãng phí vận hành. |
| **5. Success Metric** | 1. Giảm tỉ lệ hủy do thiếu xe từ ~25% → dưới 12%.<br>2. Giảm thời gian chờ trung bình từ 12 phút → dưới 7 phút.<br>3. Tỉ lệ có xe trong 3 phút từ ~60% → trên 85%. |
| **6. Operational Boundary** | AI được phép dự báo điểm nóng và draft lệnh điều xe đến trực trước 15–20'. **CẤM:** AI không được tự gửi lệnh điều xe khi chưa có Dispatcher duyệt (HITL bắt buộc); không được điều xe đến quá xa gây thiếu coverage vùng khác. |

---

## 3.3 Future-State Workflow

Quy trình tương lai — AI dự báo điểm nóng và điều xe đến trực trước:

```text
┌──────────────┐     ┌──────────────┐     ┌──────────────┐     ┌──────────────┐
│ Bước 1       │     │ Bước 2       │     │ Bước 3       │     │ Bước 4       │
│ Khách đặt xe │     │ 🔵 AI dự báo │     │ 🔵 AI draft  │     │ 🟢 Dispatch  │
│ qua app      │ ──→ │ điểm nóng +  │ ──→ │ lệnh điều xe │ ──→ │ duyệt lệnh  │
│              │     │ check giao   │     │ đến trực     │     │ → gửi tài xế │
│              │     │ thông        │     │ trước 15-20' │     │              │
│ Ai: Khách    │     │ Ai: AI Model │     │ Ai: LLM      │     │ Ai: Dispatch │
│ ⏱ 1 phút     │     │ ⏱ tự động    │     │ ⏱ tự động    │     │ ⏱ <1 phút    │
│ In: Yêu cầu  │     │ In: Lịch sử  │     │ In: Kết quả  │     │ In: Draft AI │
│ Out: Đặt xe  │     │ dữ liệu +    │     │ dự báo       │     │ Out: Xe sẵn  │
│              │     │ GPS thực     │     │ Out: Draft   │     │ sàng đúng    │
│              │     │ Out: Hotspot │     │ lệnh điều xe │     │ điểm, đúng   │
│              │     │ map          │     │ [DRAFT_ONLY] │     │ giờ          │
└──────────────┘     └──────────────┘     └──────────────┘     └──────────────┘
                                                                      │
                                                                      ▼
                                                             ┌──────────────┐
                                                             │ Bước 5       │
                                                             │ Gán xe gần   │
                                                             │ nhất, đón    │
                                                             │ dưới 7 phút  │
                                                             └──────────────┘
↩️ Fallback: Nếu AI dự báo sai → Dispatcher điều tay như quy trình cũ.
⏱ Tổng thời gian xử lý (tương lai): <3 phút/lượt
```

---

## 3.4 AI Fit Analysis

* **Kiến trúc lựa chọn:** `Rule + LLM Feature` — không cần Agent tự trị vì:
  - Quy trình dự báo có cấu trúc, rule-based theo giờ/khu vực
  - Bắt buộc HITL (Dispatcher duyệt trước khi gửi lệnh điều xe)
  - Rủi ro nếu AI sai: thiếu xe toàn khu vực → cần human override

* **Ranh giới an toàn áp dụng:**
  - Output luôn là `[DRAFT_ONLY]`, không tự gửi
  - Không điều quá 30% xe trong một vùng về một điểm
  - Fallback về điều tay nếu AI confidence < 70%

