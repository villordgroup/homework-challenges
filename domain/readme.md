# Upinus Backend Challenge - Domain

## Yêu cầu
- Giải quyết bài toán bằng Python
- Có sử dụng unittest
- Kết quả đưa lên 1 repo Github
- Có thể tự do Google, Stackoverflow, ... nhưng đừng nhờ người khác làm hộ, chúng tôi muốn xem cách bạn code và xử lý vấn đề, nó quan trọng hơn!
- Thời gian tối đa là 6h, nếu chưa xong cũng ko sao vẫn cứ gửi kết quả là link Github tới dev@upinus.com
- Mọi thắc mắc có thể contact với dev@upinus.com trực tiếp

## Đề bài:
Một trong những tính năng của Upinus là giúp kết nối các chủ cửa hàng với các nhà cung cấp (suppliers).

Trước đây, mỗi nhân viên vận hành của Upinus phải thực hiện việc chuyển tiếp các đơn hàng của các chủ cửa hàng cho các nhà cung cấp để các nhà cung cấp đóng gói và chuyển hàng đến cho các khách hàng của chủ cửa hàng. Sẽ có rất nhiều các nhà cung cấp cùng cung cấp một sản phẩm và các nhân viên vận hành sẽ phải lựa chọn nhà cung cấp phù hợp cho mỗi đơn hàng.

Đây là công việc gây mất rất nhiều thời gian nhưng các nhân viên phải làm hằng ngày. Yêu cầu đưa ra là xây dựng 1 công cụ để giúp đỡ các nhân viên vận hành.

Danh sách file cần ở chung với guideline này.

## Đầu vào:
Bao gồm 3 file csv chứa các thông tin giúp cho các nhân viên có thể chuyển tiếp đơn hàng:

- Stock: chứa các thông tin liên quan đến lượng hàng còn tồn kho với từng nhà cung cấp

- Priority: chứa các thông tin liên quan đến độ ưu tiên với từng mặt hàng của các nhà cung cấp. Với cùng 1 mặt hàng, supplier nào có độ ưu tiên càng thấp thì càng được ưu tiên khi chọn đơn hàng để chuyển tiếp (độ ưu tiên thấp nhất là 1, các supplier không được ưu tiên sẽ không được đánh độ ưu tiên)

- Order Lists: danh sách order cần chia, chứa các thông tin về sản phẩm cần đặt hàng và khách hàng cần chuyển hàng


## Đầu ra:

1. Một file zip gồm n folders để gửi cho n nhà cung cấp tương ứng và 1 file mang tên "error_order.csv" chứa các order không được phân về bất kỳ nhà cung cấp nào. Tên folder sẽ là ngày xử lý có định dạng là dd_mm_yyyy. Mỗi thư mục sẽ được đặt tên là tên của nhà cung cấp tương ứng.

2. Mỗi folder sẽ bao gồm n files csv thể hiện mỗi sản phẩm (Mỗi sản phẩm có thể bao gồm nhiều biến thể với các mã sku khác nhau nhưng sẽ có tên viết tắt như nhau) được matching với supplier đó. Tên file sẽ phải tuân thủ quy tắc sau:

```
<Product Name>_<Number Of Pieces>_<Processing Date>_<Supplier Name>
```

Trong đó:
+ __<Product Name>__ là tên viết tắt của sản phẩm.
+ __<Number Of Pieces>__ là số lượng sản phẩm được đặt hàng trong file
+ __<Processing Date>__ là ngày tiến hành xử lý có định dạng dd.mm
+ __<Supplier Name>__ là tên của nhà cung cấp sẽ xử lý

Ví dụ: Ipcase_173pcs_30.03_Alair.csv có nghĩa là:
- là file đặt hàng cho các sản phẩm có tên viết tắt là Ipcase,
- yêu cầu đặt 173 sản phẩm vào ngày 30/03
- sẽ do nhà cung cấp có tên là Alair xử lý.

3. Mỗi file csv sẽ có cấu trúc giống như Order List, mỗi hàng chứa thông tin của 1 order được gửi cho nhà cung cấp. Tuy nhiên sẽ có thêm 1 trường thông tin "Status" nằm ở cột cuối cùng chứa mã thông tin điệp xác nhận cho nhà cung cấp. Cấu trúc thông điệp status có dạng như sau:

```
<P> <a or b> <Processing Date> <Supplier Name>
```

Trong đó:
- __<a or b>__ là buổi tiến hành xử lý, a là buổi sáng, b là buổi chiều.
- __<Processing Date>__ là ngày tiến hành xử lý và có định dạng là dd/mm
- __<Supplier Name>__ là tên của nhà cung cấp sẽ xử lý đơn hàng.

Ví dụ:  `P b 30/03 Alair` thể hiện rằng đơn hàng này
- được xử lý vào buổi chiều ngày 30/03
- và được chuyển cho supplier có tên là Alair (Xem file mẫu Output_example_with_Alair.csv)

4. Logic xử lý đơn hàng:
- Các order được thanh toán trước sẽ được ưu tiên xử lý trước
- Lựa chọn các nhà cung cấp có độ ưu tiên thấp để xử lý trước, hết hàng mới đưa lên các nhà cung cấp có độ ưu tiên cao hơn.

5. Yêu cầu tính khả dụng
- Cần 1 giao diện đơn giản có ít nhất 3 input để các nhân viên có thể upload file lên xử lý
- Sau khi hệ thống xử lý xong cần trả lại file kết quả cho nhân viên
