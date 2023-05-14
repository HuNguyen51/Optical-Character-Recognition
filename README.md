# Optical-Character-Recognition
Model input: shape = (batch_size, seq_len, h, w)  
Model output shape = (batch_size, max_out_len)
# Dữ liệu
Đây dữ liệu của một <a href='https://pbcquoc.github.io/vietnamese-ocr/'>cuộc thi</a> nhận diện hình ảnh chữ viết tay thành ký tự
# Cấu trúc thư mục
Dữ liệu được để trong thư mục dataset, trong thư mục dataset có các thư mục con chứa dữ liệu train và dữ liệu test
# Những điểm cần lưu ý
- Trước khi đưa input vào model thì ta cần có bước xử lý hình ảnh đầu vào như sau:
  - Đọc ảnh, sau đó chuyển ảnh sang thang màu xám, rồi chuyển thành ảnh nhị phân bằng thư viện của openCV
  - Resize bức ảnh để có height chung (height càng lớn thì khi huấn luyện độ chính xác càng cao) mà vẫn giữ nguyên tỷ lệ
  - Bỏ những khoảng trống phía trước và phía sau của nội dung trong tấm ảnh
  - Cắt tấm ảnh ra thành nhiều tấm ảnh nhỏ hơn với ý nghĩa như 1 ảnh nhỏ sẽ đại diện cho 1 ký tự
  - Sau cùng chúng ta sẽ có được nhiều tấm ảnh nhỏ theo thứ tự từ trái sang phải của tấm ảnh
- Với input là những tấm ảnh sau khi đã được xử lý ta sẽ trích xuất đặc trưng của nó:
  - Ảnh đầu vào đưa vào model sẽ có shape là (batch_size, seq_len, h, w)  trong đó h và w là đại diện cho một tấm ản, seq_len là chuỗi các tấm ảnh nhỏ đó
  - Chúng ta cần trích xuất đặc trưng của những tấm ảnh nhỏ có kích thước h*w bằng các lớp CONDV và MaxPolling
  - Sau khi Normalization ta sẽ reshape nó để có được kích thước (batch_size, seq_len, d_model) phù hợp với đầu vào của transformer
- Với output thì là tiếng Việt, sẽ có những ký tự như ă, ắ, ơ, ớ, è, ê nên khi tạo vectorize ta phải thêm vào
- Bài này sẽ dừng ở mức độ demo nên có độ chính xác cao ở tập train nhưng tập test thì chưa do thời gian huấn luyện còn quá ít (chúng ta có thể huấn luyện lâu hơn để model đạt được hiệu quả như mong muốn)
