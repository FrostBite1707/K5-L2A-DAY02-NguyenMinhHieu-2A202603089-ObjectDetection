# Mini guideline — bài làm cá nhân

> Điền nội dung này trước khi bắt đầu gán nhãn. Giữ lại các quyết định sau khi self-QC.

## Định nghĩa lớp

| Lớp | Định nghĩa của bạn | Ví dụ/điểm phân biệt |
| --- | --- | --- |
| `car` | Xe con cá nhân (4–7 chỗ), thân liền khối phục vụ chở người | Sedan, SUV, hatchback, coupe, taxi. Khác van: đầu xe có nắp capo dài rõ rệt, thân thấp hơn |
| `truck` | Xe cơ giới chuyên chở hàng nặng, kết cấu có thùng chở hàng riêng biệt phía sau | Xe tải thùng, xe ben, đầu kéo, bán tải (pickup). Khác car/van: có thùng hàng/sàn tải hở hoặc kín tách khỏi cabin |
| `bus` | Phương tiện công cộng/dịch vụ chở khách số lượng lớn (thường từ 16–29 chỗ trở lên) | Xe buýt tuyến, xe khách, xe du lịch lớn. Thân dài, sàn cao, nhiều hàng ghế và cửa sổ kính lớn dọc thân |
| `van` | Xe dạng khối hộp (box-like), trần cao chở khách (9–16 chỗ) hoặc chở hàng nhẹ | Transit, Solati, HiAce, minivan gia đình. Khác car: đuôi xe vuông phẳng, không cốp nhô; khác truck: thân liền, không thùng tải rời |

## Quy tắc chung

- Box bám sát vật thể nghĩa là: Bounding box phải ôm khít các điểm ngoài cùng nhìn thấy được (bánh chạm đất, gương chiếu hậu, cản va). Sai số nền trống tối đa 2–3 pixel, tuyệt đối không cắt lẹm vào vỏ xe.
- Vật thể bị che khuất: Chỉ vẽ bao quanh phần thực tế nhìn thấy được (visible area). Bỏ qua không gán nhãn nếu bị che trên 70–80% hoặc chỉ còn thấy chi tiết vụn vặt.
- Vật thể bị cắt bởi rìa ảnh: Vẽ box ôm trọn phần thân xe nằm trong ảnh, cạnh box áp sát mép viền ảnh nơi xe bị cắt ngang.
- Vật thể rất nhỏ/mơ hồ: Bỏ qua vật thể nhỏ hơn 10x10px hoặc quá mờ/tối/xa không thể nhận dạng đặc trưng. Nếu phân vân giữa hai lớp, ưu tiên hình thái công năng rõ nhất (thùng hàng -> truck, khoang người liền khối -> car/van), không đoán mò.

## Ghi lại quyết định cho ít nhất ba ca mơ hồ

| Ảnh | Tình huống | Quyết định cuối | Lý do theo schema |
| --- | --- | --- | --- |
| `drive_003` |Xe bị che cắt mất 1 phần (truncated>50%) chỉ nhìn thấy phần đầu xe|Gán nhãn truck, bounding box bao trọn phần nhìn thấy chạm sát mép ảnh bên phải|Đối tượng vẫn nhận diện được dựa trên điểm chia cắt giữa đầu xe và thân cắt|
| `drive_013` |Xe bị che cắt mất 1 phần (truncated>50%) chỉ nhìn thấy phần đầu xe|Xe bị che cắt mất 1 phần (truncated>50%) chỉ nhìn thấy phần đầu xe|Đối tượng vẫn nhận diện được dựa trên đầu xe giống với các mẫu xe tải truyền thống |
| `drive_016` |Xe nhìn có vẻ là xe tải ở xa và mờ với binding box chỉ khoảng 13x24px |Xóa bỏ, không gán nhãn|Tránh tạo nhãn sai vì không đủ đặc trưng để nhận diện|
truncated