Beautiful-Trần Thu Phương-26ai.phuongtt2@vinuni.edu.vn

1. Giúp được gì
Cấu trúc hóa nhanh một ý tưởng mơ hồ: từ một prompt brainstorm chung chung ra được 5 pain point có khung rõ ràng (vấn đề — tổn thất — hướng AI).
Vai trò "CFO khắt khe" tạo áp lực phản biện thật: chỉ ra 3 lỗ hổng logic (incentive tài xế, feedback loop tự phá hoại) mà nếu tự viết một mình, dễ bị bỏ sót vì đang ở vai "người đề xuất" chứ không phải "người duyệt ngân sách".
Sửa code theo đúng nội dung file prompt_prototype.py và nội dung cấu trúc đã mô tả và gợi ý sử dụng các hàm: một khi có starter code thật (Gemini, không phải Anthropic), AI bám sát đúng cấu trúc, TODO, và style code gốc thay vì áp đặt phong cách riêng.
Debug môi trường nhanh, đúng nguyên nhân: lỗi PowerShell vs Bash là loại lỗi máy móc, AI xử lý tốt và nhanh hơn tự tra Google.
2. Sai ở đâu
 Đoán sai ý định thay vì hỏi lại khi mơ hồ
Với yêu cầu tạo 5 idea từ những chủ đề được chọn thay vì hỏi xem chọn 5 idea từ mảng nào hay mỗi idea 1 mảng thì tự gen từ đúng mảng Xanh SM

2.3. Không chủ động kiểm tra xem có "đề bài chính thức" hay không

Ở cả bước sơ đồ quy trình lẫn bước code, AI tự dựng nội dung khi lẽ ra nên hỏi thẳng: "đây là bài tập có template/đề bài cụ thể từ khóa học không?" — vì ngữ cảnh "VinUni_Codelab_Day02_Template" trong screenshot cho thấy rõ ràng đây là bài tập có khung sẵn, không phải brainstorm tự do.

3. Sửa gì cho lần sau
Hỏi 1 câu trước khi tạo tạo ra ý tưởng thay vì suy đoán
Khi thấy dấu hiệu có đề bài/khung có sẵn (tên file, cấu trúc thư mục, TODO comment) mà chưa được cung cấp, hỏi xin trước khi tự bịa nội dung thay thế.
4. Kết luận

Phần tư duy phản biện (vai CFO) là chỗ AI cộng hưởng tốt nhất — vì đó là việc "vặn lại logic" chứ không cần dữ liệu thật. Phần tạo nội dung cụ thể (số liệu, code theo đề bài) là chỗ rủi ro cao nhất — vì AI có xu hướng lấp đầy khoảng trống bằng thứ nghe hợp lý thay vì dừng lại hỏi.