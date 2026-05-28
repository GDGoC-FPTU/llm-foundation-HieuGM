# Ngày 1 — Bài Tập & Phản Ánh
## Nền Tảng LLM API | Phiếu Thực Hành

**Thời lượng:** 1:30 giờ  
**Cấu trúc:** Lập trình cốt lõi (60 phút) → Bài tập mở rộng (30 phút)

---

## Phần 1 — Lập Trình Cốt Lõi (0:00–1:00)

Chạy các ví dụ trong Google Colab tại: https://colab.research.google.com/drive/172zCiXpLr1FEXMRCAbmZoqTrKiSkUERm?usp=sharing

Triển khai tất cả TODO trong `template.py`. Chạy `pytest tests/` để kiểm tra tiến độ.

**Điểm kiểm tra:** Sau khi hoàn thành 4 nhiệm vụ, chạy:
```bash
python template.py
```
Bạn sẽ thấy output so sánh phản hồi của GPT-4o và GPT-4o-mini.

---

## Phần 2 — Bài Tập Mở Rộng (1:00–1:30)

### Bài tập 2.1 — Độ Nhạy Của Temperature
Gọi `call_openai` với các giá trị temperature 0.0, 0.5, 1.0 và 1.5 sử dụng prompt **"Hãy kể cho tôi một sự thật thú vị về Việt Nam."**

**Bạn nhận thấy quy luật gì qua bốn phản hồi?** (2–3 câu)
> Khi temperature thấp như 0.0, phản hồi thường ổn định, trực tiếp và dễ lặp lại giữa các lần gọi. Ở 0.5 câu trả lời bắt đầu tự nhiên hơn nhưng vẫn kiểm soát tốt; còn 1.0 và đặc biệt 1.5 tạo ra phản hồi đa dạng, sáng tạo hơn nhưng cũng dễ lan man hoặc thêm chi tiết kém chắc chắn. Quy luật chung là temperature càng cao thì mức độ ngẫu nhiên càng lớn, đổi lại độ nhất quán giảm.

**Bạn sẽ đặt temperature bao nhiêu cho chatbot hỗ trợ khách hàng, và tại sao?**
> Tôi sẽ đặt khoảng 0.2–0.3, hoặc 0.5 nếu chỉ chọn trong bốn mức đã thử. Chatbot hỗ trợ khách hàng cần câu trả lời nhất quán, chính xác và ít “sáng tạo” quá mức, nhưng vẫn nên đủ tự nhiên để người dùng không cảm thấy khô cứng.

---

### Bài tập 2.2 — Đánh Đổi Chi Phí
Xem xét kịch bản: 10.000 người dùng hoạt động mỗi ngày, mỗi người thực hiện 3 lần gọi API, mỗi lần trung bình ~350 token.

**Ước tính xem GPT-4o đắt hơn GPT-4o-mini bao nhiêu lần cho workload này:**
> GPT-4o đắt hơn GPT-4o-mini khoảng 33.3 lần. Workload là 10.000 người dùng x 3 lần gọi = 30.000 calls/ngày, tương đương khoảng 10,5 triệu token/ngày nếu mỗi call trung bình 350 token; vì giá input của GPT-4o là 5.00/0.150 = 33.3 lần và giá output là 20.00/0.600 = 33.3 lần so với mini, tỉ lệ tổng chi phí cũng xấp xỉ 33.3 lần. Nếu giả sử token chia đều input/output, GPT-4o khoảng $131.25/ngày còn GPT-4o-mini khoảng $3.94/ngày.

**Mô tả một trường hợp mà chi phí cao hơn của GPT-4o là xứng đáng, và một trường hợp GPT-4o-mini là lựa chọn tốt hơn:**
> GPT-4o xứng đáng khi nhiệm vụ cần lập luận sâu, độ chính xác cao hoặc xử lý yêu cầu phức tạp như phân tích tài liệu quan trọng, hỗ trợ coding khó, hoặc trả lời các câu hỏi nhiều ràng buộc. GPT-4o-mini phù hợp hơn cho workload lớn, đơn giản và lặp lại như FAQ, phân loại ticket, tóm tắt ngắn, routing yêu cầu, hoặc chatbot hỗ trợ bước đầu.

---

### Bài tập 2.3 — Trải Nghiệm Người Dùng với Streaming
**Streaming quan trọng nhất trong trường hợp nào, và khi nào thì non-streaming lại phù hợp hơn?** (1 đoạn văn)
> Streaming quan trọng nhất khi phản hồi dài hoặc người dùng đang tương tác trực tiếp, ví dụ chatbot, trợ lý viết nội dung, giải thích code, hoặc trả lời từng bước, vì người dùng thấy kết quả xuất hiện ngay và cảm giác chờ đợi giảm đáng kể. Non-streaming phù hợp hơn khi phản hồi ngắn, cần xử lý như một khối hoàn chỉnh, hoặc cần kiểm tra/validate toàn bộ output trước khi hiển thị, chẳng hạn trả về JSON, phân loại nội bộ, moderation, chấm điểm, hoặc các tác vụ chạy nền.


## Danh Sách Kiểm Tra Nộp Bài
- [x] Tất cả unit tests pass bằng mock/unittest; chạy lại `pytest tests/ -v` sau khi kích hoạt conda env và cài `requirements.txt`
- [x] `call_openai` đã triển khai và kiểm thử
- [x] `call_openai_mini` đã triển khai và kiểm thử
- [x] `compare_models` đã triển khai và kiểm thử
- [x] `streaming_chatbot` đã triển khai và kiểm thử mock; cần chạy thử tương tác sau khi có `GEMINI_API_KEY`
- [x] `retry_with_backoff` đã triển khai và kiểm thử
- [x] `batch_compare` đã triển khai và kiểm thử
- [x] `format_comparison_table` đã triển khai và kiểm thử
- [x] `exercises.md` đã điền đầy đủ
- [x] Sao chép bài làm vào folder `solution` và `solution-code` theo quy định/test suite
