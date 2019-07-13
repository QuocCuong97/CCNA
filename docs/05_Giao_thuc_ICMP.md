# Giao thức ICMP
## **1) ICMP ( Internet Control Messenger Protocol )**
### **1.1) Khái niệm**
- Do **IP** không có cơ chế để biết được dữ liệu đã được gửi đến đích hay chưa => **ICMP** ra đời
- **ICMP messenger** đơn giản chỉ có nhiệm vụ thông báo cho sender biết việc data gửi đi có vấn đề .<br>**VD :** Host A gửi 1 `packet` đến host Z , nhưng do một số nguyên nhân mà gói tin không gửi tới đúng đích.
- Các thiết bị trung gian như Router có routing protocol không đúng , được gọi là **unreachable network** .
- Cấu hình TCP/IP chưa đúng về `address` , `subnet-mask` , `default-gateway` được gọi là **unreachable host** .
- Host đích không hỗ trợ upper-layer protocol , được gọi là **unreachable protocol** .
- Host đích không hỗ trợ loại dịch vụ cần truy cập , gọi là **unreachable port/socket** .
### **1.2) Định dạng gói tin ICMP**
<img src=https://i.imgur.com/BEBEnMH.png>

- Gói tin **ICMP** :
    - **CHECKSUM** ( `16bit` ) : **ICMP** sử dụng thuận toán checksum như **IP** , nhưng **ICMP checksum** chỉ tính đến thông điệp **ICMP** .
    - **CODE** ( `8bit` ) : cung cấp thêm thông tin về kiểu thông điệp .
    - **TYPE** ( `8bit` ) : là 1 số nguyên `8 bit` để xác định thông điệp . Các giá trị của **TYPE** :
    
        | **Values** | **Meanings** |
        |------------|--------------|
        | 0 | Echo reply |
        | 3 | Destination unreachable |
        | 4 | Source quench |
        | 5 | Redirect |
        | 8 | Echo |
        | 9 | Router advertisement |
        | 10 | Router solicitation |
        | 11 | Time exceeded |
        | 12 | Parameter problem |
        | 13 | Timestamp request |
        | 14 | Timestamp reply |
        | 15 | Information request ( obsolete ) |
        | 16 | Information reply ( obsolate ) |
        | 17 | Address mask request |
        | 18 | Address mask reply |
        | 30 | Traceroute |
        | 31 | Datagram conversion error |
        | 32 | Mobile host redirect |
        | 33 | IPv6 Where-Are-You |
        | 34 | IPv6 I-Am-Here |
        | 35 | Mobile registration request |
        | 36 | Mobile registration reply |
        | 37 | Domain name request |
        | 38 | Domain name reply |
        | 39 | SKIP |
        | 40 | Photuris |
### **1.3) Các loại ICMP messenger thường thấy**
####  **ICMP echo messenger**
- Có 2 loại là **echo request** và **reply messenger** tương ứng với các trường :
    + **TYPE** = `0` => **echo request , code** = `0`
    + **TYPE** = `8` => **echo request , code** = `0`
#### **ICMP Destination Unreachable messenger**
- Nếu bị **Destination Unreachable** , thiết bị trung gian sẽ gửi về 1 **Destination Unreachable messenger** về sender .
- **Destination Unreachable** có nhiều loại ứng với các nguyên nhân khác nhau và chúng sẽ có các cặp giá trị code khác nhau .
    - Bảng code nhận dạng lỗi :

        | **Code** | **Meanings** |
        |----------|--------------|
        | 1 | Network unreachable error. |
        | 2 | Host unreachable host. |
        | 3 | Protocol unreachable error.<br>Sent when the designated transport protocol is not supported. |
        | 4 | Port unreachable error.<br>Sent when the designated transport protocol is unable to demultiplex the datagram but has no protocol mechanism to inform the sender.|
        | 5 | The datagram is too big.<br>Packet fragmentation is required but the DF bit in the IP header is set. |
        | 6 | Source route failed error. |
        | 7 | Destination network unknown error. |
        | 8 | Destination host unknown error. |
        | 9 | Source host isolated error.<br>Obsolete. |
        | 10 | The destination network is administrative prohibited. |
        | 11 | The destination host is administrative prohibited. |
        | 12 | The network is unrechable for Type Of Service. |
        | 13 | The host is unreachable for Type Of Service. |
        | 14 | Communication Administratively Prohibited.<br>This is generated if a router cannot forward a packet due to administrative filtering. |
        | 15 | Host precedence violation.<br>Sent by the first hop router to a host to indicate that a requested predence is not permitted for the particular combination of source/destination host or network,upper layer protocol,and source/destination port.
        | 16 | Precedence cutoff in effect.<br>The network operators have imposed a minimum level of precedence required for operation,the datagram was sent with a precedence below this level. |
### **ICMP Parameter Problem messenger**
- Vấn đề xảy ra khi có một vài error trong header của datagram ( ở 1 vài octet ) và không thể chuyển nó đi tiếp được . Khi đó thiết bị trung gian gửi một **ICMP Parameter Problem messenger** cho sender với trường như sau :
    - **TYPE** = `12`
    - **CODE** = `0-2`
### **ICMP Redirect / Change Request messenger**
- Là 1 loại control messenger , nó chỉ được gửi đi bởi 1 default-gateway và nó báo cho host nhận biết là có best path cho host đọc nếu có các điều kiện sau xảy ra :
    - Tại interface mà packet đã đi vào sau đó routed lại đi ra.
    - Tại subnet/network của địa chỉ IP nguồn cùng subnet/network của nexthop.
    - Khi host được để mặc định gửi là **ICMP Redirect messenger**.
- Có thể bỏ default này bằng command `no ip direct` . 
- Có các loại **Redirect Require messenger** ứng với các type và code sau :
    - **TYPE** = `5`, **CODE** = `0` => Redirect datagram for the network.
    - **TYPE** = `5`, **CODE** = `1` => Redirect datagram for the host.
    - **TYPE** = `5`, **CODE** = `2` => Redirect datagram for the type service and the network.
    - **TYPE** = `5`, **CODE** = `3` => Redirect datagram for the type service and the host.
### **ICMP Timestamp request messenger**
- Dùng để đồng bộ thời gian cho các ứng dụng giữa nơi chuyển và nơi nhận :
    - **TYPE** = `13` , **CODE** = `0` => ICMP Timestamp request messenger
    - **TYPE** = `14` , **CODE** = `0` => ICMP Timestamp reply messenger
### **ICMP Information Request and Reply Messenger**
- Để xác định số network được sử dụng
    - **TYPE** = `15` , **CODE** = `0` => ICMP Information Request messenger
    - **TYPE** = `16` , **CODE** = `0` => ICMP Information Reply messenger
### **ICMP Router Discovery messenger**
- Được dùng khi sender mất default gateway :
    - **TYPE** = `9` , **CODE** = `0` => ICMP Router Discovery messenger
    - **TYPE** = `10` , **CODE** = `0` => ICMP Router Solicitation messenger
### **ICMP Sourrce Quench messenger**
- Được dùng để báo cho sender biết có congestion và hỏi sender xem có giảm tốc độ gửi packet đi không.
- Thuộc loại Flow-Control messenger
    - **TYPE** = `4` , **CODE**= `0`
## **2) Công cụ PING**
- Là 1 công cụ phổ biến để kiểm tra xem 1 host nào đó có thể đi được qua mạng IP hay không .
- Sử dụng 2 loại gói tin :
    + **ICMP Echo Request**
    + **ICMP Echo Reply**
- PING trên PC : `C:\> ping [IP/host]`<br> => Kết quả :
    - `Reply from 8.8.8.8 : bytes=32  time<1ms TTL=128` :
        - `8.8.8.8` gửi messenger trả lời
        - `bytes=32` là kích thước gói tin được gửi đi
        - `time<1ms` là thời gian của quá trình hồi đáp
        - `TTL=128` là giá trị ***Time To Live*** của gói tin ICMP
    - `Request Time Out` : không có hồi đáp trả về 
    - `Destination host unreachable` : không thể kết nối tới mạng đích ( do định tuyến )
- PING trên Router : `Router# ping [IP/host]`<br> => Kết quả :
    - `UUUUU` : Destination host unreachable
    - `HHHHH` : Host unreachable
    - `!!!!!` : Ping Successfully
    - `?????` : Unknown packet type
    - `.....` : Timed out while waiting for a reply
## **3) Công cụ TRACERT**
- Là 1 tiện ích trên hệ điều hành Windows cho phép xác định lộ trình từ PC đến 1 đích nào đó phải đi qua các hop nào .
- Địa chỉ IP của các Router trên đường đi đều phải được hiển thị trên bảng tracert .
- Lệnh : `C:\> tracert [IP/host]`
## **4) Công cụ TRACEROUTE**
- Là 1 tiện ích của Cisco IOS có chức năng tương tự **TRACERT** của Windows .
- Lệnh : `Router# traceroute [IP/host]`


