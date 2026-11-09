# Mini guideline — bài làm cá nhân

> Điền nội dung này trước khi bắt đầu gán nhãn. Giữ lại các quyết định sau khi self-QC.

## Định nghĩa lớp

| Lớp | Định nghĩa của bạn | Ví dụ/điểm phân biệt |
| --- | --- | --- |
| `car` | Phương tiện cơ giới chủ yếu dùng để chở người (dưới 9 chỗ), thường có 4 bánh, thân xe kín, khoang hành khách và khoang động cơ. Kích thước nhỏ hoặc trung bình, thường có 2–4 cửa. Thân thấp, cabin liền thân xe|Sedan, SUV, hatchback. Có thiết kế thân xe chở người, không có thùng hàng riêng biệt. Vd: Xe biển xanh màu ghi ở drive_001.jpg|
| `truck` |Xe tải thường có cabin dành cho người lái và thùng hoặc sàn chở hàng phía sau. Kết cấu chắc chắn, kích thước và tải trọng thường lớn. |Xe tải thùng, xe ben, xe đầu kéo. Đặc điểm nổi bật là bộ phận chuyên chở hàng hóa. Vd: Xe màu đỏ chở cát ở drive_008.jpg |
| `bus` |Xe buýt cỡ lớn chở nhiều người: thân dài, nhiều cửa sổ và cửa lên xuống, kể cả xe buýt ghép nối. |Xe buýt công cộng, xe khách, xe buýt trường học. Có không gian chở khách lớn và nhiều cửa sổ. Vd: Xe nhiều cửa sổ, dài ở drive_008.jpg |
| `van` |Phương tiện cơ giới có thân xe dạng hộp, nóc cao, khoang cabin nối liền với khoang phía sau. Được sử dụng để chở người hoặc hàng hóa, thường có cửa trượt bên hông hoặc cửa mở phía sau. |Minivan, cargo van, passenger van. Thân xe kín, cao và khoang sau liền với cabin. Vd: Xe van trắng ở góc trái drive_005.jpg |

## Quy tắc chung

- Box bám sát vật thể nghĩa là: Vẽ Bounding Box bao quanh sát các cạnh ngoài cùng của vật thể, bao gồm toàn bộ phần nhìn thấy được, không lấy thừa nền, mặt đường hoặc bóng đổ.
- Vật thể bị che khuất: Vẫn gán nhãn nếu có đủ đặc điểm để nhận diện vật thể. Box chỉ bao quanh phần vật thể nhìn thấy được, không vẽ đè lên vật thể che khuất.
- Vật thể bị cắt bởi rìa ảnh: Vẫn gán nhãn nếu phần còn lại đủ để xác định loại vật thể. Box kéo sát đến rìa ảnh, không tự suy đoán phần nằm ngoài khung hình.
- Vật thể rất nhỏ/mơ hồ: Chỉ gán nhãn khi có đủ thông tin hình ảnh để xác định chính xác class. Nếu quá nhỏ, mờ hoặc không thể phân biệt, không tự ý đoán nhãn; báo lại người phụ trách theo quy định dự án.

## Ghi lại quyết định cho ít nhất ba ca mơ hồ

| Ảnh | Tình huống | Quyết định cuối | Lý do theo schema |
| --- | --- | --- | --- |
| `drive_012` |Xe buýt ghép nối 2 khoang bởi khớp xoay trùng màu với đường nên có thể bị nhầm thành 2 xe |1 box 'Bus' bao toàn bộ xe (cả 2 khoang)  |Schema quy định 1 xe là 1 box và xe bus 2 khoang là 1 phương tiện duy nhất |
| `drive_001` |Một chiếc ô tô con trắng ở đường bên phải bị cột điện che mất phần sau. |Gán 1 box car bao quanh phần nhìn thấy được. |Xe vẫn có đủ đặc điểm nhận dạng nên vẫn có thể phân biệt được là car; vật thể bị che khuất không được tách thành nhiều box. |
| `drive_005` |Xe van có hình dáng gần giống SUV, phần nóc xe cao và thân xe kéo dài. |1 box Van bao quanh toàn bộ xe |	Xe van có thân dạng hộp, nóc cao và khoang sau liền với cabin, đặc trưng của van. |
