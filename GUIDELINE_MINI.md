# Mini guideline — bài làm cá nhân

> Điền nội dung này trước khi bắt đầu gán nhãn. Giữ lại các quyết định sau khi self-QC.

## Định nghĩa lớp

| Lớp | Định nghĩa của bạn | Ví dụ/điểm phân biệt |
| --- | --- | --- |
| `car` | xe con, sedan, SUV, taxi, xe bán tải | Sedan và SUV xếp vào car. Không gán van chỉ vì xe có kích thước hơi lớn. Phân biệt bằng phần đầu xe, khoang hành khách và hình dáng giống xe con. |
| `truck` | xe tải chở hàng, xe đầu kéo | Xe tải thùng kín, tải ben, đầu kéo, bán tải chở hàng rõ ràng. Phân biệt với van ở kết cấu cabin và khoang hàng thường tách biệt hoặc mang dáng xe tải. |
| `bus` | xe buýt, minibus chở khách | Xe buýt đô thị, xe khách cỡ lớn, minibus. Phân biệt với van bằng thân dài hơn, nhiều cửa sổ hành khách và sức chứa lớn hơn. |
| `van` | xe van, minivan chở hàng hoặc chở người | Xe giao hàng, xe tải van, passenger van. Phân biệt với car bằng thân cao và vuông hơn; phân biệt với bus bằng kích thước và số hàng ghế/cửa sổ ít hơn. |

## Quy tắc chung

- Box bám sát vật thể nghĩa là: Box phải ôm sát phần phương tiện nhìn thấy được và sát với các điểm ngoài cùng, không lấy quá nhiều nền đường, vỉa hè, trời hoặc xe bên cạnh lọt vào box.
- Vật thể bị che khuất: Chỉ vẽ box quanh phần thực sự nhìn thấy, không tự đoán phần thân xe bị che khuất, chỉ gán nhãn khi phần nhìn thấy còn đủ để xác định tương đối lớp phương tiện.
- Vật thể bị cắt bởi rìa ảnh: Bounding box dừng tại đúng rìa ảnh và bao sát phần phương tiện còn nhìn thấy, chỉ nên bỏ qua khi phần còn lại quá ít, không đủ nhận diện lớp.
- Vật thể rất nhỏ/mơ hồ: Bỏ qua nếu không đủ chi tiết để xác định chắc chắn lớp hoặc không phân biệt được giữa các lớp.

## Ghi lại quyết định cho ít nhất ba ca mơ hồ

| Ảnh | Tình huống | Quyết định cuối | Lý do theo schema |
| --- | --- | --- | --- |
| `drive_002` | Các phương tiện ở quá xa, kích thước rất nhỏ và khó xác định lớp | Bỏ qua | Vật thể rất nhỏ/mơ hồ |
| `drive_003` | Các phương tiện dừng sát nhau khi chờ đèn đỏ và che khuất lẫn nhau | Chỉ label cho những phương tiện còn đủ phần nhìn thấy để xác định lớp, box bao sát phần nhìn thấy | Vật thể bị che khuất |
| `drive_004` | Chiếc xe màu đỏ ở mép dưới ảnh bị cắt mất một phần đuôi xe | Vẫn gán nhãn `car`; box bao sát phần nhìn thấy và dừng tại rìa ảnh | Vật thể bị cắt bởi rìa ảnh |
