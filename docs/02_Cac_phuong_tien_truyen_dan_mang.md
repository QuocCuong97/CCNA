# Các phương tiện truyền dẫn mạng
## **1) Các loại cáp mạng**
### **1.1) Cáp đồng trục**
- Cáp đồng trục có 2 đường dây dẫn và có chung 1 trục , một dây dẫn trung tâm ( thường là dây đồng cứng ) , đường dây còn lại tạo thành đường ống bao xuung quanh dây dẫn trung tâm ( dây dẫn này có thể là dây bên kim loại và vì có chức năng chống nhiễu nên còn gọi là lớp bọc kim) . Giữa 2 dây dẫn trên có 1 lớp cách ly , và bên ngoài cùng là lớp vỏ plastic để bảo vệ cáp .

    <img src=https://i.imgur.com/QBDQoce.png>

### **1.2) Cáp sợi quang ( Fiber-Optic Cable )**
- Cáp sợi quang bao gồm 1 dây dẫn trung tâm ( là 1 hoặc 1 bó sợi thủy tinh có thể truyền dẫn tín hiệu quang ) được bọc bằng 1 lớp vỏ bọc có tác dụng phản xạ các tín hiệu trở lại để giảm sự mất mát tín hiệu . Bên ngoài cùng là lớp vỏ plastic để bảo vệ cáp .
- Như vậy cáp sợi quang không truyền dẫn các tín hiệu điện mà truyền dẫn các tín hiệu quang ( các tín hiệu dữ liệu phải được chuyển đổi thành các tín hiệu quang và khi nhận chúng sẽ lại được chuyển đổi thành các tín hiệu quang và khi nhận chúng sẽ lại được chuyển đổi thành tín hiệu điện ) .
- Cáp quang có đường kính từ `8,3` - `100` `micron` .

    <img src=https://i.imgur.com/V0zKzap.jpg>

    <img src=https://i.imgur.com/TBrP8Fd.jpg>

- Có 2 loại cáp quang là đơn mode ( ***single mode*** ) và da mode ( ***multimode*** ) :
    - ***Multimode*** : với loại cáp quang này , nhiều mode ( hay nhiều bước sóng ) được truyền trên sợi cáp , mỗi bước sóng lan truyền theo 1 đường đi khác nhau. Cáp ***multimode*** được sử dụng chủ yếu trong các hệ thống truyền ở khoảng cách ngắn ( `<2km` ) .
    - ***Single mode*** : loại cáp quang này chỉ cho 1 mode ánh sáng được lan truyền . Cáp ***single mode***  thường được sử dụng cho khoảng cách dài và các ứng dụng tần số cao .
- Do đường kính lõi sợi thủy tinh rất nhỏ nên rất khó khăn trong việc đấu nối , nó cần công nghệ đặc biệt với kỹ thuật cao đòi hỏi chi phí cao .
- Băng thông của cáp quang có thể lên tới hàng `gbps` và cho phép khoảng cách đi cáp khá xa do độ suy hao tín hiệu trên cáp rất thấp . Ngoài ra , vì sợi cáp quang không dùng tín hiệu điện từ để truyền dữ liệu nên nó hoàn toàn không bị ảnh hưởng của nhiễm điện từ và tín hiệu truyền không thể bị phát hiện và thu trộm bởi các thiết bị khác .
- Chỉ trừ nhược điểm khó lắp đặt và giá thành cao , nhìn chung cáp quang thích hợp cho mọi mạng hiện nay và sau này .
### **1.3) Cáp xoắn đôi**
- Là loại cáp gồm 4 cặp 2 dây dẫn đồng được xoắn vào nhau làm giảm nhiễu điện từ gây ra bởi môi trường xung quanh và giữa chúng với nhau .
- Có 3 loại cáp xoắn đôi :
    - **UTP - Unshield Twisted Pair** :
        - Được cấu tạo từ *4 cặp sợi cáp xoắn vào nhau* và *1 lớp nhựa* bao bên ngoài ( có thể có *sợi dây dù* tăng khả năng chịu lực và độ bền của cáp sau khi thi công )
        - Có khả năng chống nhiễu không cao vì không có vỏ bọc chống nhiễu
        - Là loại cáp cơ bản , phổ biến nhất vì nó đáp ứng tốt yêu cầu về tốc độ truyền tải và chi phí rẻ
    - **FTP - Foil Twisted Pair** :
        - Cấu tạo gồm *lớp lá kim loại mỏng* chống nhiễu bọc quanh *4 cặp cáp xoắn* .
        - Có khả năng chống nhiễu điện từ ( **EMI** )
        - Thường được ưu tiên trong môi trường bị ảnh hưởng bởi sóng điện từ

            <img src=https://i.imgur.com/P4GREzp.png>

    - **STP - Shield Twisted Pair** :
        - Cấu tạo gồm *lớp lá kim loại* bao bọc *4 cặp cáp xoắn* và có thêm 1 lớp gồm *72 sợi kim loại đan thành lưới* ( 2 lớp chống nhiễu )
        - Có khả năng chống nhiễu điện từ cực tốt
        - Tốc độ và chiều dài tối đa của cáp **STP** đạt được giống như với cáp **UTP** . Tuy nhiên , cáp **STP** dày hơn và đắt tiền hơn
        - Được ưu tiên sử dụng tại các môi trường có độ nhiễm điện từ ( **EMI** ) cao .

            <img src=https://i.imgur.com/h5RqXkJ.png>

- Phân biệt các tiêu chuẩn **CAT 5** , **CAT 5e** , **CAT 6** :
    - **CAT 5 - Category 5** :
        - Là loại cáp mạng cơ bản nhất , gồm 2 loại :
            - **UTP** không có lá kim loại giảm nhiễu
            - **FTP** có lá kim loại giảm nhiễu
        - Có tác dụng truyền dẫn tín hiệu , thường được dùng trong mạng Ethernet 
        - Cấu tạo từ 4 cặp sợi cáp nhỏ có lõi đồng và được bao bọc bên ngoài bằng 1 lớp nhựa
        - Lõi cáp **CAT 5** thường được làm bằng đồng , thường là lõi đồng đặc ( *solid* ) hoặc lõi bện ( *stranded* ) .
        - Băng thông lên đến `100MHz`
        - Đáp ứng tốt cho các ứng dụng Ethernet tốc độ `10` - `100 Mbps`
    - **CAT 5e - Category 5 Enhanced** :
        - Gồm cả 3 loại **UTP** , **FTP** , **STP**
        - Cấu tạo từ 4 cặp sợi cáp xoắn lại với nhau theo từng cặp được bao bọc bởi lớp nhựa bảo vệ bên ngoài :
            -  Phần lõi được làm từ vật liệu dẫn điện tốt như : đồng , CCA ,...
            - Phần vỏ nhựa được chế tạo từ nhựa tốt , có khả năng uốn dẻo cao , chịa nhiệt tốt và an toàn với môi trường 
        - Cáp **CAT 5e** có lõi đặc ( *solid* ) với băng thông lên đến `100MHz` , đặc biệt được hỗ trợ **Ethernet Gigabit** tốc độ cao với khả năng truyền tải lên đến `1Gbps`
        - Đáp ứng hầu hết các nhu cầu kết nối các thiết bị trong công nghệ Bootrom , Camera và mạng máy tính Ethernet .

            <img src=https://i.imgur.com/CNlOyXq.png>

    - **CAT 6 - Category 6** :
        - Cấu tạo từ 4 cặp sợi cáp xoắn chặt vào nhau và các lớp vật lý khác tăng khả năng chống nhiễu , độ bền chắc và khả năng truyền tải .
        - Thiết kế đặc biệt với *lõi chữ thập ( central cross )* xuyên suốt chiều dài cáp mạng tách biệt hoàn toàn 4 cặp sợi cáp cho khả năng chống nhiễu chéo ( *cross-talk* ) tốt hơn .
        - Lớp vỏ thiết kế dày hơn tăng khả năng chống nhiễu điện từ ( **EMI** ) tốt hơn
        - Riêng với loại cáp mạng **CAT 6 chống nhiễu** còn được thiết kế lớp lá kim loại chống nhiễu hoặc lớp lưới kim loại chống nhiễu tăng khả năng chống nhiễu điện từ ( **EMI** ) đến tối đa , giúp cho tín hiệu truyền dẫn không bị nhiễu , đứt đoạn do các yếu tố môi trường và truyền dẫn được xa hơn .
        - Có băng thông `250MHz` và khả năng truyền tải lên tới `10 Gbps`

            <img src=https://i.imgur.com/cbm4YJv.jpg>

## **2) Chuẩn bấm hạt mạng RJ-45**

<img src=https://i.imgur.com/5jdcFfZ.jpg>

### **Chuẩn TIA/EIA 568A**
<center>

| PIN | Màu sắc | Chức năng |
|-----|---------|-----------|
| 1 | Trắng <span style="color: Chartreuse">sọc</span> xanh <span style="color: Chartreuse">lá</span> | Truyền + |
| 2 | <span style="color: Chartreuse">Xanh lá</span> | Truyền - |
| 3 | Trắng <span style="color: darkorange">sọc</span> cam | Nhận + |
| 4 | <span style="color: DodgerBlue">Xanh dương</span> | - |
| 5 | Trắng <span style="color: DodgerBlue">sọc</span> xanh <span style="color: DodgerBlue">dương</span> | - |
| 6 | <span style="color: darkorange">Cam</span> | Nhận - |
| 7 | Trắng <span style="color: sienna">sọc</span> nâu | - |
| 8 | <span style="color: sienna">Nâu | - |

</center>

### **Chuẩn TIA/EIA 568B**
<p align=center>
<table>
    <tr>
        <th>PIN</th>
        <th>Màu sắc</th>
        <th>Chức năng</th>
    </tr>
    <tr>
        <th>1</th>
        <th>Trắng <span style="color: darkorange">sọc</span> cam</th>
        <th>Truyền +</th>
    </tr>
    <tr>
        <th>2</th>
        <th><span style="color: darkorange">Cam</span></th>
        <th>Truyền -</th>
    </tr>
    <tr>
        <th>3</th>
        <th>Trắng <span style="color: Chartreuse">sọc</span> xanh <span style="color: Chartreuse">lá</span></th>
        <th>Nhận +</th>
    </tr>
    <tr>
        <th>4</th>
        <th><span style="color: DodgerBlue">Xanh dương</span></th>
        <th>-</th>
    </tr>
    <tr>
        <th>5</th>
        <th>Trắng <span style="color: DodgerBlue">sọc</span>  xanh <span style="color: DodgerBlue">dương</span></th>
        <th>-</th>
    </tr>
    <tr>
        <th>6</th>
        <th><span style="color: Chartreuse">Xanh lá</span></th>
        <th>Nhận -</th>
    </tr>
    <tr>
        <th>7</th>
        <th>Trắng <span style="color: sienna">sọc</span> nâu</th>
        <th>-</th>
    </tr>
    <tr>
        <th>8</th>
        <th><span style="color: sienna">Nâu</span></th>
        <th>-</th>
    </tr>
</table>
</p>

### **Cáp thẳng ( Straigh-Through ) và Cáp chéo ( Cross-Over )**

<img src=https://i.imgur.com/wAO4vi4.jpg>

> ## **3) Một số chuẩn Ethernet thông dụng**

<center>

| Tên thường gọi | Tốc độ | Tên chuẩn IEEE | Tên khác | Loại cáp và chiều dài |
|----------------|--------|----------------|----------|----------------------|
| Ethernet | 10Mbps | IEEE 802.3 | 10BASE-T | Cáp đồng , 100m |
| Fast Ethernet | 100Mbps | IEEE 802.3u | 100BASE-TX | Cáp đồng , 100m |
| Gigabit Ethernet | 1000Mbps<br>(1Gbps) | IEEE 802.3z | 1000BASE-LX <br>1000BASE-SX | Cáp quang , 5km (LX) <br>550m(SX)
| Gigabit Ethernet | 1000Mbps<br>(1Gbps) | IEEE 802.3ab | 1000BASE-T | Cáp đồng , 100m |

</center>

*Chú thích : **T,TX** - twisted pair ; **LX** - long-range fiber-optic ; **SX** - short-range fiber-optic*