# Giao thức TCP và UDP
>## **1) TCP ( Tranmission Control Protocol )**
- Là giao thức hướng kết nối ( connection-oriented ) nghĩa là khi muốn truyền dữ liệu thì phải thiết lập kết nối trước.
- Hỗ trợ cơ chế **Full-duplex** ( truyền và nhận dữ liệu cùng 1 lúc ).
- Cung cấp cơ chế đánh giá số gói tin ( **Sequencing** ) : để ráp gói tin cho đúng ở điểm nhận.
- Cung cấp cơ chế báo nhận ( **Acknowledement** ) : khi A gửi dữ liệu cho B, B nhận được thì gửi gói tin cho A xác nhận là đã nhận. Nếu không nhận được tin xác nhận thì A sẽ gửi cho đến khi B báo nhận thì thôi.
- Phục hồi dữ liệu bị mất trên đường truyền (A gửi mà không thấy B xác nhận sẽ gửi lại).
- **TCP Header** : do là giao thức tin cậy nên header của TCP rất phức tạp.

    ![](/images/ccna/4_Giao_thuc_TCP_va_UDP/1.png)

    - **32-bit sequence number** : dùng để đánh số thứ tự gói tin ( từ số sequence nó sẽ tính được sô byte đã được truyền)

    - **32-bit acknowledgement number** : dùng để báo nó đã nhận được gói tin nào và nó mong nhận được byte mang số thứ tự nào tiếp theo.

    - **4-bit header length** : cho biết toàn bộ header dài bao nhiêu word ( 1 word = 4 byte ).

    - **Phần ký tự** ( trước **16-bit window size** ) : là các bit dùng để điều khiển cờ (flag) ACK, cờ Sequence,...

    - **16-bit urgent pointer** : được sử dụng trong trường hợp cần ưu tiên dữ liệu ( kết hợp với bit điều khiển u r g ở trên).

    - Các trường ở trên là cố định, trường **Options** để lập trình thêm các tính năng cho TCP nếu có nhu cầu.

> ## **2) UDP ( User Datagram Protocol )**
- Là loại giao thức connectionless ( nghĩa là có gói tin nào đẩy ngay vào đường truyền mà không cần thiết lập kết nối trước ).
- Không đảm bảo tính tin cậy khi truyền dữ liệu và không có cơ chế phục hồi dữ liệu ( nó không quan tâm gói tin có đến đích hay không, không biết gói tin có bị mất mát trên đường đi hay không ).
- UDP được sử dụng khi tốc độ là mong muốn và sửa lỗi là không cần thiết. VD : UDP thường được sử dụng cho chương trình phát sóng trực tiếp và trò chơi trực tuyến
- **UDP Header** 

    ![](/images/ccna/4_Giao_thuc_TCP_va_UDP/2.png)

    - **16-bit source port, 16-bit destination port** : tầng Transport dùng 1 cặp **source port** và **destination port** để định danh 1 session đâng truy nhập vào đường truyền của kết nối UDP. Ta có thể coi port là địa chỉ của tầng Transport ( giao thức DNS chạy UDP port 53, TFTP port 69,...).
    
    - **16-bit UDP length** : cho biết toàn bộ gói tin UDP dài tổng cộng bao nhiêu byte. Ta thấy 16-bit thì sẽ có tổng cộng 2^16 byte = 65536 giá trị ( từ 0 -> 65536 ).

    - **16-bit UDP checksum** : sử dụng thuật toán mã vòng CRC để kiểm soát lỗi ( chỉ kiểm tra 1 cách hạn chế ).

- Các ứng dụng sử dụng UDP là : VoIP, video conference, DNS, TFTP,...
> ## **3) Tiến trình bắt tay 3 bước ( Three-way handshake )**

![](/images/ccna/4_Giao_thuc_TCP_va_UDP/3.png)

### **Giải thích**
- **B1** : Host A gửi cho host B 1 gói tin có cờ **SYN** được bật, với số thứ tự được đánh là 100.

- **B2** : Host B nhận được gói tin thì B gửi lại gói tin có cờ **SYN** được bật lên, kèm theo đó là cờ **ACK** ( để xác nhận ).

    **ACK=101** nghĩa là " này A, tôi đã nhận được gói tin có **SEQ=100**, tôi mong muốn nhận được thêm gói tin có **SEQ=101** ". Khi gửi gói tin đi nó sẽ đánh số thứ tự **SEQ=300**

- **B3** : Sau khi kết nối đã được thiết lập thì A gửi gói tin để đáp ứng nhu cầu của B.
    Gói tin được đánh số **SEQ=101** : để đáp ứng nhu cầu của B

    **ACK=301** dùng để báo là đã nhận được gói tin có **SEQ=300**. Và chỉ có cờ **ACK** được bật lên bởi gói tin bước 3 được dùng để báo nhận cho gói tin bước 2.

### **Cơ chế điều khiển luồng trong TCP ( TCP Flow Control)**

![](/images/ccna/4_Giao_thuc_TCP_va_UDP/4.jpg)

- Giả sử : Sender gửi quá nhiều dữ liệu cho Receiver, thì Receiver sẽ chuyển vào bộ đệm để chờ xử lý, đến lúc bộ đệm đầy thì B gửi tín hiệu cho A để không truyền nữa cho đến khi B xử lý hết thì sẽ gửi lại gói tin cho A để tiếp tục nhận dữ liệu.
### **Fixed Windowing**

![](/images/ccna/4_Giao_thuc_TCP_va_UDP/5.jpg)

- Thay vì gửi từng byte rồi đợi **ACK** thì sender sẽ gửi nhiều byte cùng lúc ( **Window Size** bằng bao nhiêu sẽ gửi bấy nhiêu ).
- Ở cơ chế **Fixed Windowing** thì **Window Size** cố định, nhưng có trường hợp không thể giữ cố định được.
### **TCP Sliding Windowing ( Window Size có thể thay đổi)**

![](/images/ccna/4_Giao_thuc_TCP_va_UDP/6.jpg)

- **Window Size = 3** nên Sender sẽ gửi lần lượt 3 byte nhưng Receiver chỉ nhận được 2 byte ( do nghẽn mạng, không xử lý nổi ) thì Receiver sẽ **ACK=3** để yêu cầu Sender gửi lại byte thứ 3 đồng thời nó cũng báo hãy sử dụng **Window Size = 2** ( vì nó chỉ chịu nổi size = 2). Sender sau đó sẽ set **Window Size = 2**.
> ## **4) Truyền dữ liệu Half-duplex và Full-duplex**
- Trên một môi trường truyền dẫn ( VD trên 1 sợi cáp đồng ), thông tin lan truyền giữa các thiết bị mạng có thể được thực hiện theo nhiều dạng thức khác nhau như : chỉ cho phép truyền 1 chiều ( quá trình t1 ) từ thiết bị mạng này tới thiết bị mạng khác trong 1 đơn vị thời gian, quá trình t2 chỉ được thực hiện khi quá trình t1 kết thúc. Dạng thức này gọi là **Half-duplex**. Trong trường hợp môi trường truyền và các thiết bị mạng có thể hoạt động song song cùng lúc để quá trình t1 và t2 xảy ra đồng thời ta có dạng thức truyền **Full-duplex**.

    - **Half-duplex** : giữa 2 đường truyền dữ liệu và luồng tin, chỉ truyền theo 1 hướng tại 1 thời điểm khi 1 thiết bị hoàn thành việc truyền dẫn, nó phải chuyển môi trường truyền đến thiết bị khác. Một thiết bị có thể đóng vai trò THU và PHÁT tín hiệu nhưng tại 1 thời điểm nó chỉ có thể thực hiện 1 vai trò duy nhất.**VD**: Hoạt động của bộ tọa đàm điện thoại, mạng LAN có sử dụng các thiết bị trung tâm là thiết bị Layer 1 thì luôn sử dụng **Half-duplex**.
    
        ![](/images/ccna/4_Giao_thuc_TCP_va_UDP/8.png)
    - **Full-duplex** : cho phép dữ liệu truyền đồng thời trên cả 2 đường, mỗi thiết kế sẽ có 1 kênh riêng. Mỗi thiết bị có thể đồng thời vừa PHÁT lại vừa THU tín hiệu. Các modem máy tính đều hoạt động theo phương thức này, mạng LAN sử dụng toàn thiết bị tập trung Layer 2 hoặc 2 máy tính kết nối trực tiếp với nhau đều có thể sử dụng.

        ![](/images/ccna/4_Giao_thuc_TCP_va_UDP/7.png)

    - Bên cạnh đó, có thể áp dụng dạng truyền **Simple Mode**. Thông tin chỉ truyền theo 1 chiều quy định trước, một thiết bị chỉ đóng vai trò THU hoặc PHÁT cố định. Hệ thống báo cháy dùng phương thức này

    
    

