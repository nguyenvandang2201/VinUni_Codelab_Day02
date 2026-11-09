# 01 — Problem Scan & Quick Problem Cards

> **Học viên:** Nguyễn Văn Đăng — **MSSV:** 2A202603371
> **Lab 02:** AI Product Scoping — Vin Smart Future
>
> Ghi chú: các con số thời gian và tần suất dưới đây là **ước tính cho bài lab**, chưa phải số đo thực tế tại công ty thành viên. Cần xác minh lại bằng dữ liệu vận hành trước khi dùng cho quyết định thật.

---

# 🔍 Phase 1 — SCAN

Quét vận hành các công ty thành viên Vingroup bằng cả 4 lens: **Lặp lại**, **Tốn thời gian**, **AI có thể tốt hơn**, **Pain từ người khác**.

| # | Subsidiary | Lens | Mô tả ngắn bài toán |
|---|------------|------|---------------------|
| 1 | **VinFast** | AI có thể tốt hơn | Khách tả lỗi xe bằng lời tự do (*"đi qua gờ giảm tốc kêu cụp cụp bánh trước"*). Cố vấn dịch vụ phải gọi lại hỏi thêm, tự tra tài liệu để chọn nhóm hệ thống và mã lỗi sơ bộ. Mất khoảng 12 phút/yêu cầu, khoảng 20% phiếu bị xếp sai nhóm. |
| 2 | **Vinhomes** | Lặp lại | Phản ánh của cư dân trên app Vinhomes Resident (mất nước, thang máy, tiếng ồn, gửi xe) được nhân viên CSKH đọc và chuyển tay đến đúng tổ kỹ thuật hoặc ban quản lý tòa. Hàng trăm phiếu/ngày ở mỗi khu đô thị lớn. |
| 3 | **Vinpearl** | Tốn thời gian | Email đặt phòng theo đoàn (group booking) từ công ty lữ hành viết rất khác nhau: nhiều hạng phòng, nhiều ngày, yêu cầu đặc biệt. Sales phải đọc, đối chiếu quỹ phòng rồi soạn báo giá, mất 30–45 phút/email. |
| 4 | **Vinmec** | Pain từ người khác | Bệnh nhân không biết nên khám chuyên khoa nào. Tổng đài viên hỏi triệu chứng theo kinh nghiệm, đôi khi đặt sai khoa, bệnh nhân phải chuyển khoa và chờ thêm. |
| 5 | **Xanh SM** | Pain từ người khác | Tài xế phàn nàn điểm đón trong các khu đô thị lớn (ví dụ Vinhomes Ocean Park) không chính xác. Ghim GPS rơi vào sai cổng/sai tòa, tài xế và khách phải gọi qua lại 2–3 cuộc mỗi chuyến. |
| 6 | **VinUni** | Lặp lại | Trợ giảng trả lời đi trả lời lại các câu hỏi giống nhau về cài môi trường, API key và lỗi autograder trên GitHub Classroom trong mỗi buổi lab. |

---

# 🃏 Phase 2 — QUICK-ASSESS: 3 Quick Problem Cards

Chọn top 3: **#1 (VinFast — chẩn đoán sơ bộ lỗi xe)**, **#2 (Vinhomes — điều hướng phản ánh cư dân)**, **#4 (Vinmec — gợi ý chuyên khoa)**.

### Card #1 — VinFast: Chẩn đoán sơ bộ lỗi xe từ mô tả tiếng Việt của khách

```text
┌─ QUICK PROBLEM CARD #1 ───────────────────────────────────────────────
│
│ Bài toán (1 câu): Biến mô tả lỗi xe bằng lời tự do của khách thành
│   phiếu tiếp nhận có nhóm hệ thống + mã lỗi sơ bộ + mức độ an toàn.
│ Công ty thành viên: [x] VinFast (Xưởng dịch vụ / Đại lý ủy quyền)
│
│ Ai đang đau (Actor)? Cố vấn dịch vụ (Service Advisor — SA): quá tải
│   giờ cao điểm. Khách hàng: phải nghe gọi lại nhiều lần, chờ lâu.
│   Kỹ thuật viên (KTV): nhận phiếu ghi chung chung, phải chẩn đoán lại.
│
│ Workflow thủ công hiện tại (5 bước):
│   1. Tổng đài ghi mô tả vào CRM (text tự do)
│   ──> 2. SA đọc ticket, gọi lại khách hỏi thêm triệu chứng
│   ──> 3. SA tra tài liệu kỹ thuật, chọn nhóm hệ thống + mã lỗi
│   ──> 4. SA đánh giá an toàn: cứu hộ ngay hay xếp lịch?
│   ──> 5. SA tạo phiếu DMS, xếp khoang & KTV
│
│ Bước nào tốn thời gian/lỗi nhất? Bước 2 + 3 (9 phút/lượt), và bước 4
│   (chỉ 1 phút nhưng không có checklist → dễ bỏ sót ca nguy hiểm).
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2–4: trích xuất triệu
│   chứng, gợi ý câu hỏi làm rõ, đề xuất top-3 nhóm lỗi, gắn cờ an toàn.
│
│ Đo thành công bằng gì (Metric có số)?
│   - Thời gian SA xử lý 1 yêu cầu: 12 phút ──> dưới 4 phút
│   - Đúng nhóm hệ thống (top-1) ≥ 85% so với kết luận cuối của KTV
│   - Bỏ sót ca an toàn (phanh/lái/pin cao áp/khói): 0 ca
│
│ Quick Architecture: [ ] No AI  [x] Rule  [x] LLM  [ ] Agent
│   (Rule làm lưới an toàn bắt từ khóa nguy hiểm + LLM phân loại/draft)
└───────────────────────────────────────────────────────────────────────
```

**Tự phản biện (vai CFO / Trưởng phòng vận hành):**
- *Có cần AI không?* Rule từ khóa ("phanh", "khói") bắt được ca an toàn rõ ràng, nhưng không hiểu được *"kêu cụp cụp khi qua gờ"* (hệ thống treo) hay *"vô lăng rung khi lên 80"* (cân bằng lốp / lái). Tiếng lóng và câu phủ định là chỗ LLM hơn rule.
- *Metric có đo được không?* Có. DMS lưu kết luận cuối của KTV, dùng làm nhãn để so với đề xuất của AI.
- *Rủi ro lớn nhất:* AI nói "xe an toàn, cứ đi" với một ca lỗi phanh. Vì vậy AI không được kết luận an toàn với khách, và lớp rule bắt từ khóa nguy hiểm chạy **trước** LLM.

### Card #2 — Vinhomes: Phân loại & điều hướng phản ánh cư dân

```text
┌─ QUICK PROBLEM CARD #2 ───────────────────────────────────────────────
│
│ Bài toán (1 câu): Tự động phân loại phản ánh của cư dân trên app
│   Vinhomes Resident và chuyển đúng tổ xử lý của từng tòa.
│ Công ty thành viên: [x] Vinhomes
│
│ Ai đang đau (Actor)? Nhân viên CSKH ban quản lý (đọc & chuyển tay);
│   cư dân (chờ phản hồi lâu khi phiếu bị chuyển sai tổ).
│
│ Workflow thủ công hiện tại (4 bước):
│   1. Cư dân gửi phản ánh (text + ảnh) trên app
│   ──> 2. CSKH đọc, xác định loại sự cố & tòa
│   ──> 3. CSKH chuyển phiếu cho tổ kỹ thuật / an ninh / vệ sinh
│   ──> 4. Tổ xử lý nhận phiếu, phản hồi cư dân
│
│ Bước nào tốn thời gian/lỗi nhất? Bước 2–3 (khoảng 4 phút/phiếu; phiếu
│   gửi ngoài giờ hành chính phải chờ tới sáng hôm sau mới được chuyển).
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2: phân loại + gắn mức
│   khẩn cấp (mất nước toàn tòa > bóng đèn hành lang hỏng).
│
│ Đo thành công bằng gì (Metric có số)?
│   - Thời gian từ lúc gửi đến lúc tới đúng tổ: ~4 giờ ──> dưới 15 phút
│   - Tỉ lệ phiếu bị chuyển sai tổ: < 5%
│
│ Quick Architecture: [ ] No AI  [x] Rule  [ ] LLM  [ ] Agent
└───────────────────────────────────────────────────────────────────────
```

**Tự phản biện:** App đã có ô chọn loại sự cố. Chỉ cần bắt buộc chọn loại, cộng thêm bảng tra *loại × tòa → tổ xử lý*, thì phần lớn phiếu đã route được mà không cần LLM. LLM chỉ hữu ích cho phần còn lại (cư dân chọn "Khác" hoặc chọn sai), nên làm Rule trước và đo lại.

### Card #3 — Vinmec: Gợi ý chuyên khoa khi đặt lịch khám

```text
┌─ QUICK PROBLEM CARD #3 ───────────────────────────────────────────────
│
│ Bài toán (1 câu): Từ mô tả triệu chứng của bệnh nhân khi đặt lịch,
│   gợi ý chuyên khoa phù hợp để tổng đài viên xếp lịch đúng ngay lần đầu.
│ Công ty thành viên: [x] Vinmec
│
│ Ai đang đau (Actor)? Tổng đài viên đặt lịch; bệnh nhân bị xếp sai khoa
│   phải chờ chuyển khoa.
│
│ Workflow thủ công hiện tại (4 bước):
│   1. Bệnh nhân gọi / chat mô tả triệu chứng
│   ──> 2. Tổng đài viên hỏi thêm theo kinh nghiệm
│   ──> 3. Chọn chuyên khoa & bác sĩ còn lịch
│   ──> 4. Xác nhận lịch hẹn qua SMS
│
│ Bước nào tốn thời gian/lỗi nhất? Bước 2–3 (khoảng 6 phút/cuộc; triệu
│   chứng chồng lấn như đau ngực: tim mạch hay hô hấp?).
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2: gợi ý câu hỏi sàng lọc
│   + top-2 chuyên khoa; nhận diện dấu hiệu cấp cứu để chuyển ngay.
│
│ Đo thành công bằng gì (Metric có số)?
│   - Tỉ lệ bệnh nhân phải chuyển khoa sau lần khám đầu: giảm 30%
│   - Thời gian mỗi cuộc gọi đặt lịch: 6 phút ──> dưới 4 phút
│
│ Quick Architecture: [ ] No AI  [ ] Rule  [x] LLM  [ ] Agent
└───────────────────────────────────────────────────────────────────────
```

**Tự phản biện:** Đây là lĩnh vực y tế. Gợi ý sai khoa với một ca cấp cứu (đau ngực do nhồi máu cơ tim) có thể gây hậu quả nghiêm trọng. Bài toán cần quy trình sàng lọc lâm sàng được bác sĩ phê duyệt, cùng dữ liệu đã ẩn danh theo quy định bảo vệ dữ liệu cá nhân, trước khi bàn tới AI.

---

## 🗳️ Lựa chọn cho Deep-Dive

Chọn **Card #1 — VinFast: Chẩn đoán sơ bộ lỗi xe** để làm Deep-Dive (xem [02-deep-dive-report.md](02-deep-dive-report.md)).

| Card | Quyết định | Lý do |
|---|---|---|
| #1 VinFast | ✅ **Deep-Dive** | Ngôn ngữ tự do (tiếng lóng, mô tả âm thanh) là chỗ LLM hơn rule rõ rệt. Có sẵn nhãn để đánh giá (kết luận cuối của KTV trong DMS). Rủi ro kiểm soát được bằng HITL (SA duyệt mọi phiếu) và lưới an toàn bằng rule. |
| #2 Vinhomes | ❌ Rule trước | Phần lớn giá trị đạt được bằng form bắt buộc chọn loại + bảng tra route. Chưa đủ lý do để trả chi phí và rủi ro của LLM. |
| #3 Vinmec | ⏸ Chưa phù hợp | Rủi ro lâm sàng cao. Cần quy trình sàng lọc được bác sĩ phê duyệt và khung tuân thủ dữ liệu y tế trước. |
