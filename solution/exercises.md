# K3 — Ngày 1: Bài Tập & Phản Ánh
## Khám Phá LLM API | Phiếu Thực Hành

**Thời lượng:** 9h00–13h00
**Cách làm:** Trả lời từng câu ngay sau khi hoàn thành block tương ứng,
đừng để dồn hết về cuối buổi. Thay dòng để trống bên dưới mỗi câu hỏi
bằng câu trả lời thật, chấm tự động sẽ đếm số câu đã trả lời.

---

## Block 1 — API Cơ Bản (trả lời sau Checkpoint 1)

### Câu 1.1 — Độ nhạy của temperature
Gọi `call_openai` với temperature 0.0, 0.5, 1.0 và 1.5 dùng prompt
**"Hãy kể cho tôi một sự thật thú vị về Việt Nam."**

**Bạn nhận thấy quy luật gì qua bốn phản hồi?** (2–3 câu)
> Ở cả bốn mức temperature, model đều chọn cùng một sự thật nổi bật là hang Sơn Đoòng, hang động tự nhiên lớn nhất thế giới, cho thấy với câu hỏi mang tính kiến thức phổ biến thì temperature không làm đổi nội dung cốt lõi được chọn. Sự khác biệt chủ yếu nằm ở cách diễn đạt, temperature thấp cho câu trả lời có cấu trúc rõ ràng và mạch lạc hơn, còn temperature càng cao thì câu chữ càng biến đổi tự do hơn ở cách hành văn dù thông tin chính vẫn giữ nguyên.

### Câu 1.2 — Chọn temperature cho sản phẩm
**Bạn sẽ đặt temperature bao nhiêu cho chatbot hỗ trợ khách hàng, và tại sao?**
> Em sẽ chọn temperature thấp, khoảng từ 0.0 đến 0.3, vì chatbot hỗ trợ khách hàng cần trả lời nhất quán, chính xác và có thể dự đoán được, tránh việc model tự sáng tạo thêm thông tin sai lệch về chính sách, giá cả hay quy trình, điều này quan trọng hơn nhiều so với việc câu trả lời đa dạng và phong phú.

### Câu 1.3 — Đánh đổi chi phí
Kịch bản: 10.000 người dùng hoạt động mỗi ngày, mỗi người gọi API 3 lần,
mỗi lần trung bình ~350 token đầu ra.

**Ước tính GPT-4o đắt hơn GPT-4o-mini bao nhiêu lần cho workload này? Nêu một
trường hợp GPT-4o xứng đáng với chi phí và một trường hợp nên dùng mini:**
> Với 30.000 lượt gọi mỗi ngày và mỗi lượt sinh khoảng 350 token đầu ra, tổng chi phí GPT 4o vào khoảng 105 đô la mỗi ngày trong khi GPT 4o mini chỉ khoảng 6.3 đô la, tức GPT 4o đắt hơn gần 17 lần. GPT 4o xứng đáng dùng khi tác vụ cần suy luận phức tạp và độ chính xác cao như phân tích hợp đồng pháp lý hoặc debug code khó, còn GPT 4o mini phù hợp hơn cho các tác vụ đơn giản khối lượng lớn như phân loại tin nhắn hoặc trả lời câu hỏi thường gặp, nơi chênh lệch chất lượng không đáng để trả chi phí cao hơn nhiều lần như vậy.

---

## Block 2 — System Prompt & Token (trả lời sau Checkpoint 2)

### Câu 2.1 — Sức mạnh của persona
Gọi `chat_with_system_prompt` hai lần với cùng câu hỏi
**"Giải thích blockchain là gì?"** nhưng hai system prompt khác nhau:
- "Bạn là giáo viên tiểu học, giải thích thật đơn giản cho trẻ 8 tuổi."
- "Bạn là chuyên gia tài chính, trả lời chuyên sâu bằng thuật ngữ kỹ thuật."

**Hai phản hồi khác nhau như thế nào (độ dài, từ vựng, ví dụ)? System prompt
ảnh hưởng đến hành vi model ra sao?** (3–4 câu)
> Với persona giáo viên tiểu học, phản hồi dài khoảng 380 từ, dùng ẩn dụ đời thường như so sánh blockchain với việc cả nhóm bạn cùng ghi chép vào sổ đổi nhãn vở, câu ngắn và không thuật ngữ. Với persona chuyên gia tài chính, phản hồi dài tới 1077 từ, gần gấp ba lần, dùng dày đặc thuật ngữ kỹ thuật như SHA 256, ECDSA, Proof of Work, Proof of Stake, Merkle Tree và Byzantine Fault Tolerance, cấu trúc phân tích theo từng mục chuyên sâu. Điều này cho thấy system prompt không chỉ đổi giọng văn mà còn đổi cả độ sâu kiến thức, độ dài và mức độ thuật ngữ chuyên ngành mà model chọn sử dụng, dù nội dung cốt lõi về khái niệm blockchain vẫn không đổi.

### Câu 2.2 — tiktoken vs đếm từ
Chọn một đoạn văn tiếng Việt ~100 từ. So sánh số token theo `count_tokens`
(tiktoken) với ước lượng `số từ / 0.75` mà Part 1 đã dùng.

**Hai con số chênh nhau bao nhiêu phần trăm? Vì sao tiếng Việt thường tốn
nhiều token hơn tiếng Anh cùng độ dài?**
> Với đoạn văn 122 từ, tiktoken đếm được 138 token trong khi ước lượng số từ chia cho 0.75 cho ra 163 token, chênh nhau khoảng 18 phần trăm. Cách ước lượng số từ chia 0.75 vốn được hiệu chỉnh cho tiếng Anh, nơi phần lớn từ chỉ là một token, nên khi áp dụng cho tiếng Việt nó không phản ánh đúng cách tokenizer thực sự hoạt động. Tiếng Việt có nhiều dấu thanh và ký tự Unicode tổ hợp như â, ă, ê, ô, ơ, ư kèm dấu, nên một từ tiếng Việt thường bị bộ mã hóa tách thành nhiều token hơn so với một từ tiếng Anh tương đương về nghĩa, khiến chi phí token thực tế cho tiếng Việt thường cao hơn ước lượng ban đầu.

---

## Block 3 — Streaming & Độ Bền (trả lời sau Checkpoint 3)

### Câu 3.1 — Trải nghiệm người dùng với streaming
**Streaming quan trọng nhất trong trường hợp nào, và khi nào thì
non-streaming lại phù hợp hơn?** (1 đoạn văn)
> Streaming quan trọng nhất trong các ứng dụng hội thoại tương tác trực tiếp như chatbot hay trợ lý ảo, nơi người dùng đang chờ và nhìn thấy phản hồi xuất hiện dần giúp giảm cảm giác chờ đợi, đặc biệt với các câu trả lời dài như ví dụ persona chuyên gia tài chính ở câu 2.1 mất gần 15 giây mới xong, nếu không stream thì người dùng sẽ nhìn màn hình trống suốt quãng thời gian đó. Ngược lại, non streaming phù hợp hơn khi kết quả cần được xử lý tiếp trước khi hiển thị, chẳng hạn parse JSON, kiểm duyệt nội dung, hoặc dùng trong pipeline tự động và xử lý hàng loạt, lúc đó nhận toàn bộ phản hồi một lần rồi xử lý sẽ đơn giản và ít lỗi hơn nhiều so với việc ghép các mảnh chunk lại giữa chừng.

### Câu 3.2 — Vì sao backoff theo cấp số nhân?
**So với delay cố định (ví dụ luôn chờ 1 giây), exponential backoff có lợi
thế gì khi API bị quá tải? Điều gì xảy ra nếu hàng nghìn client cùng retry
với delay cố định giống nhau?**
> Exponential backoff tăng dần thời gian chờ sau mỗi lần thất bại theo công thức delay gốc nhân với 2 mũ số lần thử, giúp giảm áp lực lên server đang quá tải thay vì dội thêm request ngay lập tức và cho server có thời gian phục hồi. Nếu dùng delay cố định, hàng nghìn client bị lỗi cùng lúc sẽ đồng loạt retry sau đúng cùng một khoảng thời gian, tạo ra một đợt sóng request dồn dập đánh sập server lần nữa ngay khi nó vừa kịp hồi phục, lặp đi lặp lại thành vòng lặp quá tải triền miên, trong khi backoff theo cấp số nhân giúp giãn đều các lần retry ra theo thời gian và tránh được hiện tượng này.

---

## Block 4 — Mini-Project (trả lời sau Checkpoint 4)

### Câu 4.1 — Thiết kế persona
**Bạn chọn persona gì cho trợ lý của mình? Viết lại system prompt đó và giải
thích 1–2 lựa chọn từ ngữ quan trọng trong prompt (ví dụ: vì sao yêu cầu
"trả lời ngắn gọn", vì sao chỉ định ngôn ngữ...):**
> Persona được chọn là bạn là trợ giảng thân thiện của khóa AI, trả lời ngắn gọn bằng tiếng Việt, đây cũng là persona đang dùng ở entry point của run_assistant. Từ ngắn gọn quan trọng vì trợ lý CLI chạy qua terminal với streaming, câu trả lời quá dài vừa tốn token và chi phí vừa khiến người dùng phải chờ lâu hơn để đọc hết, còn việc chỉ định rõ bằng tiếng Việt quan trọng vì nếu không ràng buộc thì model có thể lẫn lộn ngôn ngữ trả lời tùy theo ngôn ngữ của câu hỏi trước đó trong lịch sử hội thoại.

### Câu 4.2 — Hạn chế & cải thiện
**Trợ lý của bạn hiện có hạn chế lớn nhất là gì (ví dụ: history chỉ 3 lượt,
không có bộ nhớ dài hạn, không kiểm duyệt nội dung...)? Đề xuất một cải
thiện cụ thể và mô tả ngắn cách triển khai:**
> Hạn chế lớn nhất là history chỉ giữ lại 6 tin nhắn gần nhất tương đương 3 lượt hỏi đáp, nên trợ lý quên hoàn toàn ngữ cảnh của các lượt hội thoại trước đó, nếu người dùng nhắc lại điều đã nói ở lượt đầu tiên khi đang ở lượt thứ năm thì model sẽ không biết. Cải thiện cụ thể là thêm một bước tóm tắt, mỗi khi history vượt quá 6 tin nhắn thì gọi API một lần để tóm tắt các lượt cũ thành một đến hai câu ngắn, lưu vào một biến tóm tắt dài hạn riêng và luôn chèn tóm tắt đó vào đầu danh sách messages ngay sau system prompt ở mỗi lượt gọi tiếp theo, vừa giữ được ngữ cảnh dài hạn vừa không để messages phình to không kiểm soát.

---

## Danh Sách Kiểm Tra Nộp Bài

- [ ] `python grade.py` — xem điểm tự động, mục tiêu ≥ 75/100
- [ ] Cả 4 checkpoint pytest đều pass
- [ ] Tất cả 9 câu trong file này đã được trả lời
- [ ] Đã copy bài làm vào folder `solution/` và zip theo hướng dẫn README
