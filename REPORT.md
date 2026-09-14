# Báo cáo — Ngày 2: phát hiện vật thể

**Họ và tên:** Nguyễn Trọng Thắng<br>
**MSSV:** 2A202602169<br>
**Hình thức:** cá nhân<br>
**Mã cặp:** SOLO

## 1. Bài độc lập và nguồn dữ liệu

- Mã SHA-256 của ZIP ảnh được cấp:F7D99888F21440FB0374D84962B93213BD8C14E665D093CC8D37F4C61B71ED33
- Bốn mã ảnh:['drive_022', 'drive_033', 'drive_038', 'drive_008']
- Số vật thể thực tế:54
- Mã SHA-256 của gói YOLO của bạn:572dfbd49be6b42362379eb6cd10b6f2fe62ff6150b39f6deb7ad85980faf97d
- Mã SHA-256 của gói CVAT gốc của bạn:f24602b598f9602062850caaa7441440e2c0d9afd04e41b292e52da2bcb0df9a
- Nguồn đối chiếu:bộ tham chiếu do người hướng dẫn thực hành cấp
- Mã SHA-256 của gói đối chiếu:c8bbc767d8bb9a29f4ca5abf0c3516e5c2af94c58143a980b0148cfe0b500d2b
- Nếu làm cá nhân, ghi mã lần phát và thời điểm nhận bộ tham chiếu:ngày 14/9

Giải thích vì sao bài của bạn vẫn độc lập trước khi đối chiếu:
Toàn bộ quá trình gán nhãn dữ liệu trên CVAT, thiết lập quy tắc, xuất gói nhãn và huấn luyện mô hình YOLO đều được thực hiện hoàn toàn độc lập dựa trên tư duy cá nhân và tập dữ liệu gốc được cấp. Không có sự tham khảo, trao đổi hay sao chép nhãn từ bộ tham chiếu hoặc dữ liệu đối chiếu trước khi hoàn thành xong bước tự đánh giá.

## 2. Quyết định phân lớp

| Ảnh/vật thể | Lớp | Dấu hiệu nhìn thấy | Quy tắc áp dụng |
| --- | --- | --- | --- |
| drive_022 (vật thể #3) | Car | Thân xe ô tô rõ nét, có 4 bánh, kính chắn gió và kích thước chuẩn tỷ lệ xe ô tô| Gán nhãn Car cho mọi phương tiện cơ giới 4 bánh chở người quan sát được từ góc nhìn camera giao thông. |

Nêu một ví dụ cho thấy lớp và thuộc tính là hai loại thông tin khác nhau:
Lớp (Class): Xác định bản chất đối tượng là gì (Ví dụ: Car - ô tô).

Thuộc tính (Attribute): Mô tả thêm đặc điểm trạng thái của đối tượng đó không làm thay đổi bản chất lớp (Ví dụ: Màu sắc của xe là màu trắng, hoặc trạng thái xe là đang di chuyển).

## 3. Tự kiểm tra và sửa nhãn

| Trước khi sửa | Loại lỗi | Cách phát hiện | Sau khi sửa và quy tắc |
| --- | --- | --- | --- |
| Hộp nhãn bao trùm thiếu phần gương và cản xe | hình học | Kiểm tra bằng mắt thường qua khung bounding box thấy viền bám chưa khít biên vật thể | Kéo giãn hộp bounding box ôm trọn toàn bộ viền thực tế của vật thể theo quy tắc bám sát biên. |

- Số hộp `needs_review` trước và sau khi kiểm:Trước kiểm: 6 hộp | Sau kiểm: 0 hộp
- Một quyết định chưa đủ bằng chứng và cách bạn xin hỗ trợ:Gặp vật thể bị che khuất một phần lớn phía sau góc khuất bóng râm khó phân biệt rõ ranh giới thuộc lớp xe tải hay xe con; cách xử lý là ghi chú trạng thái needs_review và tham vấn Lab Coach quy tắc định biên cho xe bị khuất quá 50%.

## 4. Một dòng nhãn YOLO

- Dòng `class x_center y_center width height`:0 0.26857 0.50965 0.11195 0.08695
- Tên lớp và tọa độ điểm ảnh `xyxy`:lớp=0 (Car) | pixel xyxy: [136.1, 298.3, 207.7, 354.0]
- Vì sao dòng đúng định dạng vẫn có thể sai lớp, phạm vi hoặc hình học?
Định dạng dòng YOLO chỉ đảm bảo cấu trúc cú pháp đúng chuẩn (gồm số thứ tự lớp và 4 giá trị tọa độ chuẩn hóa từ 0 đến 1). Tuy nhiên, về mặt ngữ nghĩa nội dung vẫn có thể sai nếu gán nhãn nhầm tên lớp (ví dụ xe tải nhưng gán nhãn xe con), hoặc vẽ hộp bounding box quá rộng/quá hẹp so với ranh giới thực tế của vật thể.

## 5. Huấn luyện và dự đoán thử

- Ba mã ảnh huấn luyện:['drive_022', 'drive_033', 'drive_038']
- Mã ảnh thẩm định:['drive_008']
- Mô tả một dự đoán trong `detect_result.jpg`:Mô hình nhận diện chính xác vị trí xe ô tô (Car) với độ tin cậy (confidence score) cao (>0.85), bounding box ôm sát thân xe trên ảnh drive_008.
- Dự đoán đó gợi ý cần kiểm lại quy tắc hoặc dữ liệu nào?Gợi ý quy tắc định biên các xe ở khoảng cách xa khung hình cần được duy trì nhất quán.
- Minh chứng nào có thể bác bỏ nhận định của bạn? Sự xuất hiện của các trường hợp dự đoán nhầm lẫn lớp (false positive) hoặc bỏ sót đối tượng (false negative) khi kiểm tra trên tập dữ liệu lớn hơn.
- Vì sao kết quả trên bốn ảnh không phải phép đánh giá mô hình dùng thực tế? Vì tập dữ liệu quá nhỏ (chỉ gồm 4 ảnh chia tách train/val) không đủ độ bao phủ đa dạng các tình huống thực tế (điều kiện ánh sáng, thời tiết, góc chụp khác nhau), dẫn đến hiện tượng quá khớp (overfitting) và không đại diện cho độ tổng quát hóa của mô hình trên tập dữ liệu lớn.

## 6. Đối chiếu nhãn

- Số hộp ghép được:39
- IoU trung bình và trung vị:trung bình=0.763158, trung vị=0.792243
- Mức đồng thuận lớp:0.666667
- Số hộp phía bạn không ghép được:15
- Số hộp phía đối chiếu không ghép được:11
- Một điểm khác biệt cụ thể:Sự chênh lệch biên độ bounding box ở các vật thể ở xa do cách căn chỉnh điểm mút góc khác nhau giữa nhãn cá nhân và nhãn tham chiếu.
- Quy tắc hoặc hành động sửa phát sinh:Thống nhất lại quy tắc cắt mép biên cho các vật thể có kích thước nhỏ ở xa khung hình camera.
- Vì sao mức đồng thuận cao không chứng minh mọi nhãn đều đúng?Mức đồng thuận cao chỉ phản ánh sự trùng khớp cao giữa hai nguồn gán nhãn với nhau; nếu cả hai nguồn cùng áp dụng sai một quy tắc (ví dụ cùng bỏ sót các vật thể bị che khuất một phần), mức đồng thuận vẫn cao nhưng nhãn thực tế vẫn chưa hoàn toàn chính xác.

## 7. Kiểm tra kho GitHub cá nhân

- [x] Có phiếu quy tắc với ba tình huống mơ hồ.
- [x] Có kết quả kiểm hai gói xuất.
- [x] Có thông tin lần huấn luyện và ảnh dự đoán.
- [x] Có tóm tắt, bảng và ảnh phủ của bước đối chiếu.
- [x] Không có gói xuất thô, bộ nhãn tham chiếu hoặc trọng số mô hình.
- [x] Không có dữ liệu VinFast/khách hàng/ảnh cá nhân/mật khẩu/mã truy cập.

Minh chứng mạnh nhất trong bài và câu hỏi còn lại cho Lab Coach:
Sự thống nhất logic xuyên suốt từ mã SHA gói dữ liệu, kết quả thực nghiệm huấn luyện mô hình YOLO độc lập cho đến bộ số liệu đối chiếu chi tiết.