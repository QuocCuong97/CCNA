# SNMP - Simple Network Management Protocol
## **1) Giới thiệu**
- Để có thể bao quát được hiện trạng mạng của một doanh nghiệp, đội ngũ quản trị phải sử dụng một hệ thống quản lý tập trung . Các thông số kỹ thuật của tất cả các thiết bị như tải CPU, băng thông sử dụng trên các đường truyền, dung lượng bộ nhớ, tình trạng up/down của các cổng,... sẽ được tập hợp về các server quản lý nằm tại trung tâm điều hành mạng **NOC** ( ***Network Operating Center*** ) cả doang nghiệp giúp đội ngũ quản trị nắm rõ về tình trạng hiện tại của mạng .
- Nền tảng của việc quản lý mạng tập trung là một giao thức cho phép truyền tải các thông số cần quản lý từ các thiết bị về một đầu mối quản lý trung tâm, giao thức này có tên là **SNMP -** ***Simple Network Management Protocol*** .
## **2) Mô hình giao tiếp SNMP**
- **SNMP** quy định một cấu trúc thông tin đơn giản, chuẩn hóa, cho phép sắp xếp một cách hệ thống thông tin thu được và dễ dàng hiển thị các thông tin này trên một server tập trung .
- Thiết bị quản lý và server quản lý giao tiếp với nhau bằng **SNMP** theo mô hình sau :

    <img src=https://i.imgur.com/ZiIpX4g.png>

- Với **SNMP**, các thiết bị được quản lý ( router, switch, firewall, server,....) được gọi là các **Agent** ; server quản lý tập trung được gọi là **NMS - Network Management System** . Việc trao đổi thông tin bằng **SNMP** giữa **NMS** và **Agent** được tiến hành theo cách thức như sau :
    - Khi **NMS** cần thông tin gì trên **Agent** ( VD : tải trên 1 đường link, tải CPU, dung lượng bộ nhớ,...), **NMS** sẽ phát đi một thông điệp `get` gửi đến **Agent** để yêu cầu thông tin này. Bên cạnh đó, khi **NMS** muốn thay đổi thông số kỹ thuật nào đó trên **Agent**, nó xung sẽ gửi cho **Agent** một thông điệp `set` cho thông số muốn thay đổi .
    - **Agent** khi nhận được `get` sẽ hồi đáp lại các gói **SNMP** chứa các thông tin mà **NMS** quan tâm. Hoạt động hồi đáp này là kết quả của hoạt động truy vấn từ **NMS** . **Agent** không tự động gửi thông tin của mình lên **NMS** mà chỉ trả lời những truy vấn nhận được từ **NMS** .
    - Hiện thực **SNMP** trên các **Agent** có thể được bật chế độ gửi cảnh báo về **NMS** khi một sự kiện nào đó xảy ra . Các cảnh báo này được gọi là `Trap` . Khác với hành động hồi đáp ở trên, `trap` do **Agent** chủ động gửi về cho **NMS** . Hoạt động `trap` của **Agent** không cần hồi đáp từ **NMS** .
- Giao thức **SNMP** chạy trên nền **UDP**, sử dụng port `161` và port `162` . Trong đó :
    - Port `161` sử dụng cho hoạt động truy vấn - hồi đáp ( `get`, `set`, trả lời cho `get` ) 
    - Port `162` sử dụng cho hoạt động `Trap` 
## **3) Object, SMI và MIB**
- Mỗi thông số kỹ thuật của **Agnet** sẽ được lưu vào một biến số gọi là **object** . Khi **NMS** cần thông tin về một thông số nào đó của **Agent**, nó sẽ truy vấn đến **object** tương ứng của thông số ấy .
- Các **object** đến lượt chúng lại được tổ chức và sắp xếp vào một cơ sở dữ liệu gọi là **MIB -** ***Management Information Base*** . Cách thức xây dựng các **object** và tổ chức thành **MIB** được **IETF** quy định trong các tài liệu ***RFC 1155 (SMIv1 - Structure of Management Information Version 1)*** và ***RFC 2578 (SMIv2 - Structure of Management Information Version 2)*** . Dựa trên các phương thức này, các nhà sản xuất có thể định nghĩa ra các file **MIB** chứa các **object** mô tả các thông số kỹ thuật cho sản phẩm của mình . 
- Tổ chức **IETF** định nghĩa ra 1 file **MIB** có chuẩn tên gọi là **MIB II** ( ***RFC 1213*** ) , bao gồm các **object** mô tả các thông số thường gặp nhất trên các thiết bị như tốc độ cổng, MTU, số lượng byte gửi/nhận trên cổng, bộ nhớ, CPU,.... Hầu hết mọi sản phẩm hỗ trợ **SNMP** đều tích hợp file **MIB II** .
- Bên cạnh đó, với các thông số kỹ thuật trên sản phẩm do mình sản xuất mà chưa được mô tả trong **MIB II**, các hãng cũng tự xây dựng các file **MIB** riêng cho các đối tượng này. **VD :** **Cisco** đã công bố chính thức hàng trăm file **MIB** cho các sản phẩm của mình .
- Để truy vấn được một **object** nào của **Agent**, **NMS** phải được tích hợp file **MIB** có chứa **object** ấy . Các chương trình **NMS** hiện nay đều được tích hợp hàng ngàn file **MIB** của nhiều hãng khác nhau để có thể thực hiện quản lý thiết bị của các hãng này .
## **4) Xác thực với SNMP**
- **NMS** và **Agent** xác thực nhau trong việc trao đổi thông tin **SNMP** bằng cách sử dụng các password đơn giản gọi là ***community***. 
- Có 3 loại ***community*** :
    - `read - only (ro)`
    - `read - write (rw)`
    - `trap`
- Các ***community*** `read-only` và `read-write` được sử dụng để xác thực quyền truy nhập của **NMS** đến **Agent** :
    - Với `read - only`, **NMS** chỉ được quyền đọc ( `get` ) chứ không được quyền sửa ( `set` ) các **object** trên **Agent**
    - Với `read-write`, **NMS** được cả 2 quyền đọc và sửa giá trị của các **object** trên **Agent** .
- ***Community*** `trap` được sử dụng để **NMS** xác thực quyền gửi `Trap` của **Agent** . Chỉ **Agent** nào gửi `trap` với ***community*** hợp lệ, **NMS** mới tiếp nhận `trap` của **Agent** ấy .
## **5) Các version của SNMP**
- Hiện nay có 3 version của **SNMP** đang được sử dụng :
    - **SNMP version 1** (*hay SNMPv1*) : được định nghĩa trong ***RFC 1157*** của **IETF** . **SNMPv1** sử dụng 3 loại ***community*** đã đề cập ở trên để xác thực . **SNMPv1** không có bất kỳ cơ chế mã hóa nào với thông tin trao đổi .
    - **SNMP version 2** (*hay SNMPv2*) : được định nghĩa trong ***RFC 1441*** nhưng chưa được ủy ban của **IETF** thông qua vì các thành viên của ủy ban chưa thống nhất với nhau về cách thức bảo mật và quản trị của **SNMPv2**
        - Trong thời gian đó, một phiên bản khác của **SNMP** được định nghĩa trong ***RFC 1901*** là **community - based SNMPv2** ***(SNMPv2c)*** . **SNMPv2c** chính là **SNMPv2** nhưng sử dụng phương thức bảo mật với **community** giống như của **SNMPv1** . **SNMPv2c** ngày càng phổ biến và được sử dụng rộng rãi trên các sản phẩm có hỗ trợ **SNMP** .
        - **SNMPv2** đinh nghĩa thêm một số loại gói tin mới so với **SNMPv1** cho phép hoạt động truy vấn thông tin được hiệu quả hơn. Bên cạnh đó, **SNMPv2** còn mở rộng kích thước của các biến số trong các **object** từ 32bit lên 64bit cho phép các **object** có thể có dải giá trị lớn hơn .
        - Ngoài ra, bên cạnh hoạt động `Trap`, **SNMPv2** còn định nghĩa thêm hoạt động `Inform` . `Inform` cũng là hoạt động phát cảnh báo từ **Agent** về **NMS**, tuy nhiên khác với **Trap**, `Inform` được hồi đáp từ **NMS** .
    - **SNMP version 3  (*hay SNMPv3*)** : được định nghĩa trong các ***RFC*** từ ***3410*** đến ***3415*** . Nhược điểm về bảo mật của 2 version 1 và 2 là chỉ sử dụng các ***community*** clear text để xác thực và không mã hóa dữ liệu . **SNMPv3** khắc phục điều này bằng cách đưa vào **SNMPv3** các cơ chế bảo mật giúp xác thực một cách an toàn và thực hiện mã hóa dữ liệu cần trao đổi . **SNMPv3** định nghĩa 3 mức độ (level) bảo mật :
        - `NoAuthNoPriv` : Không xác thực, không mã hóa .
        - `AuthNoPriv` : Xác thực nhưng không mã hóa. Các phương thức xác thực được sử dụng gồm : `HMAC-MD5` và `HMAC-RSA`
        - `AuthPriv` : Xác thực và mã hóa . Xác thực được thực hiện giống như trên . Mã hóa được thực hiện bằng kỹ thuật mã hóa `DES`, `3-DES` hay `AES`
## **6) Cấu hình SNMP**