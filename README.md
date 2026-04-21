Dự án này xây dựng một hệ thống tự động hóa sử dụng n8n để kết nối Telegram Bot, Google Gemini AI và Google Drive. Hệ thống cho phép người dùng gửi ảnh qua Telegram, sau đó AI sẽ phân tích và lưu trữ ảnh lên Drive với tên file chính là nội dung phân tích.

Hướng dẫn chi tiết các bước thiết lập
Bước 1: Thiết lập Telegram Trigger
Tạo Bot qua @BotFather trên Telegram để lấy Access Token.

Trong n8n, thêm node Telegram Trigger.

Chọn sự kiện: On message.

Kết nối Credential bằng Token đã lấy.

Bước 2: Tải dữ liệu hình ảnh (Get a File)
Thêm node Telegram với hành động Get File.

Tại ô File ID, sử dụng Expression: {{ $json.message.photo.last().file_id }} để lấy ảnh chất lượng cao nhất.

Bật tùy chọn Download để tải file về hệ thống dưới dạng Binary.

Bước 3: Phân tích hình ảnh với Gemini AI
Sử dụng node Google Gemini với hành động Analyze Image.

Input Type: Chọn Binary File(s).

Input Data Field Name: Điền data (tên mặc định từ node Get File).

Text Input: Yêu cầu AI mô tả ngắn gọn (ví dụ: "Mô tả ảnh này trong 10 từ tiếng Việt").

Bước 4: Lưu trữ lên Google Drive
Thêm node Google Drive hành động Upload.

File Name: Sử dụng kết quả từ Gemini: {{ $node["Analyze an image"].json.text.substring(0, 1000) }}.jpg.

Input Data Field Name: Chuyển sang Expression và trỏ về dữ liệu Binary của node tải file: {{ $node["Get a file"].binary.data }}.

Bước 5: Phản hồi kết quả
Thêm node Telegram (Send Message) để thông báo cho người dùng kèm đường dẫn hoặc tên file đã lưu.
