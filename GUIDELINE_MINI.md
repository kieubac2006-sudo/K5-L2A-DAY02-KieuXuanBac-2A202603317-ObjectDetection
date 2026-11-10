# Mini guideline — bài làm cá nhân

> Điền nội dung này trước khi bắt đầu gán nhãn. Giữ lại các quyết định sau khi self-QC.

## Định nghĩa lớp

| Lớp | Định nghĩa của bạn | Ví dụ/điểm phân biệt |
| --- | --- | --- |
| `car` | Xe con, sedan, SUV, taxi, xe bán tải.|Các loại xe cá nhân phổ thông, kích thước nhỏ. Phân biệt với van ở chỗ xe bán tải có thùng nhỏ vẫn tính là car. |
| `truck` | Xe tải chở hàng, xe đầu kéo. | Xe có thùng chở hàng lớn phía sau hoặc phần đầu kéo rõ ràng. |
| `bus` | Xe buýt, minibus chở khách. | Xe cỡ lớn, dài, có nhiều cửa sổ kính hai bên dành cho hành khách.|
| `van` | Xe van, minivan chở hàng/người | To hơn xe con (car) nhưng nhỏ hơn xe buýt. Liền khối, không có thùng tách rời như truck. |

## Quy tắc chung

- Box bám sát vật thể nghĩa là: Vẽ các cạnh của bounding box chạm khít vào mép ngoài cùng (trên, dưới, trái, phải) của phần xe có thể nhìn thấy được trong ảnh, bao gồm cả gương chiếu hậu.
- Vật thể bị che khuất: Vẫn vẽ bounding box bao quanh phần nhìn thấy được của xe, miễn là phần đó đủ đặc điểm để nhận diện đúng lớp (car, truck...).
- Vật thể bị cắt bởi rìa ảnh: Vẽ bounding box bám theo phần xe xuất hiện trong khung hình (cạnh box sẽ nằm sát mép ảnh).
- Vật thể rất nhỏ/mơ hồ: Bỏ qua không gán nhãn nếu xe ở quá xa, hình ảnh bị nhòe đến mức không thể xác định chắc chắn đó là xe gì (không thỏa mãn đủ đặc điểm nhận diện)

## Ghi lại quyết định cho ít nhất ba ca mơ hồ

| Ảnh | Tình huống | Quyết định cuối | Lý do theo schema |
| --- | --- | --- | --- |
| `drive_001` |Một chiếc xe van bị xe khác che khất gần như cả xe phía dưới, chỉ thấy phần trên | Gán nhãn là `van` và chỉ bọc phần nhìn thấy. | Theo schema, xe van chở hàng nhỏ thuộc lớp `van`. Dù bị che vẫn nhận diện được vì lớn hơn xe con và nửa sau xe khoang vuông rộng rãi. |
| `drive_036` | Một chiếc xe giống xe 16 chỗ nhưng ở khá xa, hơi mờ. | Gán nhãn là `bus`. | Theo schema, minibus chở khách được xếp vào lớp `bus`. |
| `drive_031` | Một xe chở hàng liền khối cỡ trung bình, phân vân giữa van và truck. | Gán nhãn là `van`. | Xe liền khối, không có thùng tách rời rõ rệt như `truck`. |
