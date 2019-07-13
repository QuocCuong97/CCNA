# VPN - Virtual Private Network
## **1) Khái niệm VPN**
- Là mạng riêng mở rộng cho phép sử dụng hạ tầng được chia sẻ của 1 nhà cung cấp dịch vụ để triển khai 1 mạng dành riêng cho doanh nghiệp .

    <img src=https://i.imgur.com/QWqyyOG.png>

## **2) Phân loại VPN**
- **Overlay VPN** : là công nghệ trong đó nhà cung cấp dịch vụ cung cấp các đường link ảo **point-to-point** đấu nối giữa các chi nhánh của khách hàng . **Overlay VPN** có thể bao gồm các công nghệ lớp 2 như **X.25** , **Frame-Relay** , **ATM** hay các công nghệ lớp 3 như **GRE VPN** , **IP Sec VPN** ,...
- **Peer-to-Peer VPN** : là công nghệ trong đó nhà cung cấp dịch vụ tham gia vào quá trình định tuyến cho khách hàng . **VD :** **MPLS-VPN** .
## **3) IP Sec VPN**
- Là 1 Framework cho phép sử dụng nhiều phương pháp bảo mật khác nhau để bảo vệ gói tin IP khi chúng phải đi qua 1 môi trường mạng không an toàn .
- **IP Sec** cung cấp các dịch vụ sau trong việc bảo mật dữ liệu :
    - *Mã hóa dữ liệu ( **data confidentiality** )* : dữ liệu sẽ được mã hóa bởi các thuật toán mã hóa tin cậy để tránh việc bị đọc trộm hoặc đánh cắp nội dung trên đường di chuyển .
    - *Toàn vẹn dữ liệu ( **data integrity** )* : dữ liệu sẽ được bảo vệ chống lại việc bị thay đổi nội dung trên đường đi .
    - *Xác thực dữ liệu ( **data authentication** )* : dữ liệu sẽ được xác thực nguồn gốc khi nhận được tại phía nhận để đảm bảo rằng dữ liệu này đến từ đúng thiết bị gửi mong muốn .
- **IP Sec** sử dụng nhiều kỹ thuật bảo mật được chuẩn hóa :

     <img src=https://i.imgur.com/VIhuUGw.jpg>
     
    - *Giao thức **IP Sec*** ( **IP Protocol** ) được sử dụng có thể là **ESP** hoặc **AH** hoặc kết hợp cả 2 giao thức này .
    - *Kỹ thuật mã hóa dữ liệu* ( **encryption** ) được sử dụng có thể là **DES** , **3DES** , hay **AES** .
    - *Kỹ thuật xác thực* có thể là **MD5** hoặc **SHA** .
    - *Phương pháp trao đổi phát sinh key mã hóa / xác thực* có thể là thuật toán **DH1** , **DH2** hoặc **DH5** .
- Kỹ thuật **IP Sec** thường được sử dụng để xây dựng các kết nối **VPN** giữa các chi nhánh của 1 mạng doanh nghiệp qua môi trường Internet ( ***Site-to-Site*** ) hoặc giữa 1 người dùng đầu cuối với hệ thống mạng nội bộ của doanh nghiệp mình ( ***Client-to-Site*** ) .
    - Với ***Site-to-Site VPN*** , doanh nghiệp có thể xây dựng các đường **VPN** đấu nối giữa các chi nhánh qua môi trường Internet để dự phòng cho các đường truyền **WAN** . Đây là 1 giải pháp đấu nối dự phòng tiết kiệm chi phí , có tính bảo mật và mở rộng cao .

        <img src=https://i.imgur.com/Rb66SIT.png>

    - Giải pháp ***Client-to-Site VPN*** cho phép người dùng đầu cuối có thể truy cập vào trong môi trường mạng nội bộ khi đang ở ngoài Internet . ***Client-to-Site VPN*** thực hiện bảo mật dữ liệu người dùng truy xuất từ mạng doang nghiệp giúp cho hoạt động này được an toàn và hiệu quả .

        <img src=https://i.imgur.com/n8qwZcp.png>

## **4) GRE VPN - General Routing Protocol**
<img src=https://i.imgur.com/eu5doHP.png>

- **GRE** là 1 kỹ thuật đường hầm ở Layer 3 cho phép đóng gói dữ liệu của nhiều loại giao thức khác nhau như **IP** , **IPX** , **AppleTalk** , các giao thức định tuyến ,... để truyền tải qua 1 mạng IP .
- Khi thực hiện đóng gói , mặc định **GRE** thêm vào 1 **overhead** `24 byte` gồm `20 byte` **IP header** và `4 byte` **GRE header** .

    <img src=https://i.imgur.com/9qm8WfX.png>

- **GRE** không thực hiện bất kỳ cơ chế bảo mật nào cho dữ liệu được đóng gói ( không mã hóa , không xác thực ,... ) .
- Tuy nhiên , **GRE** đa năng hơn **IP Sec** nhiều trong việc truyền tải dữ liệu của các giao thức khác nhau , trong đó , nổi bật nhất là **GRE** hỗ trợ truyền tải thông tin các giao thức định tuyến . Từ đó , các Router ở 2 đầu đường hầm **GRE** có thể chạy định tuyến với nhau thông qua **GRE Tunnel** .
### **Cấu hình GRE Tunnel**
<img src=https://i.imgur.com/2QQ5sZO.png>

- Cấu hình R1 và R2 :
    ```
    R1(config) # interface tunnel 12
	R1(config-if) # tunnel source f0/1
	R1(config-if) # tunnel destination 200.0.0.1
	R1(config-if) # tunnel mode gre ip
	R1(config-if) # ip address 192.168.12.1 255.255.255.0
	R1(config-if) # exit

	R2(config) # interface tunnel 12
	R2(config-if) # tunnel source f0/1
	R2(config-if) # tunnel destination 100.0.0.1
	R2(config-if) # tunnel mode gre ip
	R2(config-if) # ip address 192.168.12.2 255.255.255.0
	R2(config-if) # exit
    ```

=> **GRE Tunnel** nối giữa 2 chi nhánh R1 và R2 được thiết lập dựa trên 2 địa chỉ IP Public `100.0.0.1` và `200.0.0.1` 
- Kiểm tra cấu hình **Tunnel** :
    ```
    R# show interface tunnel 12
    ```
    - Trên R1 :

        <img src=https://i.imgur.com/3cOZYQr.png>

    - Trên R2 :

        <img src=https://i.imgur.com/qSsed7j.png>

=> Sau khi **tunnel** được thiết lập, sơ đồ đấu nối giữa R1 và R2 có thể được trình bày lại như sau :

<img src=https://i.imgur.com/Y6FmKAq.png>

- Vì **GRE Tunnel** có hỗ trợ định tuyến, có thể cấu hình 1 hình thức định tuyến bất kỳ qua sơ đồ này để 2 mạng LAN của 2 chi nhánh có thể thấy nhau :
    ```
    R1(config) # router eigrp 100
	R1(config-router) # network 192.168.1.0
	R1(config-router) # network 192.168.12.0
	
	R2(config) # router eigrp 100
	R2(config-router) # network 192.168.2.0
	R2(config-router) # network 192.168.12.0
    ```
=> Mối quan hệ láng giềng **EIGRP** có thể được thiết lập qua `Tunnel 12` và định tuyến hội tụ :

- Trên R1: 

    <img src=https://i.imgur.com/MJsYg7O.png>

- Trên R2 :

    <img src=https://i.imgur.com/I9leWHn.png>

=> 2 mạng **LAN** đã có thể đi đến nhau qua **tunnel** :

<img src=https://i.imgur.com/vFnt1km.png>

=> Như vậy bằng cách thiết lập **GRE VPN** qua môi trường Internet , hai chi nhánh của doanh nghiệp đã có thể chạy định tuyến và thông suốt dữ liệu giữa các mạng **LAN** với nhau.