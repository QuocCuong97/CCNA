# Leased-line Point-to-Point
## **1) Mô hình kết nối**
- Khi thuê **Leased-line** , khách hàng giống như được cấp 1 đoạn cáp thật để đấu nối giữa 2 Router tại 2 chi nhánh .
- Đường truyền vật lý thực sự :
- Tại phía khách hàng , các Router của khách hàng đấu nối bằng cáp `serial` đến 1 thiết bị của nhà cung cấp dịch vụ gọi là **CSU / DSU** ( **Channel Service Unit / Data Service Unit** ) .
- Từ **CSU / DSU** , nhà cung cấp dịch vụ tiếp tục kéo cáp truyền dữ liệu từ khách hàng về mạng **WAN** của mình , đoạn cáp này gọi là `Local Loop` .
- Nhiệm vụ của **CSU / DSU** là thực hiện chuyển đổi định dạng tín hiệu phát ra từ Router khách hàng thành định dạng tín hiệu thích hợp trên đường `Local Loop` nối về nhà cung cấp .
- **CSU / DSU** còn được gọi là **DCE - Data Communication Equipment** và Router phía khách hàng được gọi là **DTE - Data Terminal Equipment** .
- Kỹ thuật truyền dữ liệu trên đường `serial` từ **DTE** đến **DCE** thường là kỹ thuật truyền nối tiếp đồng bộ . Kỹ thuật này đòi hỏi phải có 1 thiết bị tạo xung đồng hồ ( **xung clock** ) cho hoạt động đồng bộ tín hiệu , thiết bị này chính là **DCE** . **DCE** sẽ thực hiện cấp xung clock xuống cho **DTE** để đảm bảo thực hiện tác vụ truyền dữ liệu .
- Việc truyền số liệu trên dây `serial` giữa **DTE** và **DCE** có thể được thực hiện bởi nhiều chuẩn khác nhau như **V.35** , **X.21** , **EIA/TIA-232** ,... trong đó phổ biến nhất là **V.35** .
- Các dòng Router phổ biến của Cisco như **2800** , **2900** thường hỗ trợ 2 loại cổng `serial` là **WIC-1T** và **WIC-2T** . Các card **WIC-1T** và **WIC-2T** là những card được gắn vào các slot chuyên dùng cho giao tiếp **WAN** của Router .
## **2) Giao thức Layer 2 chạy trên leased-line Point-to-Point**

### **2.1) HDLC - High Data Level Control**
- **HDLC** thực hiện chức năng ở layer 2 , hỗ trợ liên kết dữ liệu ở dạng Frame . Đảm bảo việc gửi dữ liệu đến người dùng được thực hiện chính xác .
- Xử lý dữ liệu theo `bit` , nghĩa là dữ liệu được kiểm tra theo từng `bit` . Dữ liệu truyền chỉ gồm các `bit` nhị phân và không chứa bất kỳ mã điều khiển đặc biệt nào . Thông tin trong 1 Frame chứa những lệnh điều khiển và lệnh đáp ứng .
- Hỗ trợ truyền ***Full-Duplex*** , trong đó dữ liệu được truyền theo 2 hướng tại cùng thời điểm , nhờ đó công suất cao hơn .
- Mặc định , khi 2 cổng `serial` của Router Cisco được kết nối **leased-line** với nhau , giao thức **HDLC** được sử dụng trong kết nối này . Khi được `no shutdown` , các cổng tự động chạy **HDLC** .
- Một cổng `serial` trên Router Cisco được kết nối **leased-line** có thể ở 1 trong các trạng thái sau :
<center>

| <center>Status | <center>Line Protocol | <center>Meaning |
|--------|---------------|---------|
| Administrative down | Down | Cổng chưa `no shutdown` |
| Down | Down | Cổng đứt kết nối vật lý |
| Up | Down | + Chưa cấp xung clock <br> + Keepalive không đúng / tắt <br> + Không khớp giao thức |
| Up | Up | Hoạt động bình thường |

</center>

- Tốc độ danh định ( `bandwidth` ) của cổng `serial` được Cisco IOS thiết lập là `1,544 Mbps` .
### **Cấu hình HDLC trên cổng Serial**
<img src=https://i.imgur.com/qpdAA9i.png>

- Bật **HDLC** ( trong trường hợp cổng đang chạy giao thức khác ) :
    ```
    R1(config) # interface s0/0
    R1(config-if) # no shutdown
    R1(config-if) # encapsulation hdlc

    R2(config) # interface s0/0
    R2(config-if) # no shutdown
    R2(config-if) # encapsulation hdlc
    ```
- Thay đổi giá trị `bandwidth` :
    ```
    R(config) # interface s0/0
    R(config-if) # bandwidth 512
    R(config-if) # exit
    ```
- Kiểm tra giao thức
    ```
    R# show interface s0/0
    ```

### **2.2) PPP - Point-to-Point Protocol**
- Chức năng tương tự **HDLC** nhưng có nhiều điểm ưu việt hơn .
- **PPP** gồm 2 thành phần là **LCP** và **NCP** . Việc chia thành 2 module giúp cho hoạt động của **PPP** được chuyên biệt hóa , tăng tính linh hoạt cũng như mở rộng đáng kể các tính năng của **PPP** :
    - **LCP - Link Control Protocol** : là thành phần thực hiện mọi chức năng layer 2 của **PPP** như thiết lập và giải phóng các liên kết layer 2 , thực hiện các thao tác xác thực , thực hiện các thủ tục nén **PPP** , xây dựng các đường ***multilink***,...Nhờ có **LCP** , **PPP** mở rộng 1 cách đáng kể những tính năng layer 2 có thể thực hiện được trên đường link .
    - **NCP - Network Control Protocol** : **PPP** sử dụng 1 loạt các module **CP** ( **Control Protocol** ) để tương tác với các giao thức lớp trên . <br>**VD :** **PPP** có module **IPCP** để tương tác với giao thức **IPv4** , **IPv6CP** để tương tác **IPv6** , **CDPCP** để tương tác **CDP** .
- **PPP** là 1 giao thức khá phổ biến , được sử dụng đặc biệt nhiều trên các **leased-line point-to-point** .
- Còn được sử dụng trong các giải pháp **Data Link** như **PPPoE** , **PPPoA** , **PPPoFR** ,...

### **Xác thực PAP và CHAP với PPP**
- Trên đường link chạy **PPP** , một trong 2 Router sẽ đứng ra xác thực Router còn lại : Router bị xác thực phải khai báo chính xác 1 cặp username / password để có thể thiết lập được đường link đến đầu kia ; nếu không khai báo đúng username / password , trạng thái đường link sẽ không thể ***up*** được và cổng sẽ ở trạng thái ***up/down*** .
- **PPP** sử dụng 2 phương pháp xác thực là **PAP** và **CHAP** .
#### **PAP - Password Authentication Protocol**
- **PAP** sử dụng cơ chế bắt tay 2 bước ( ***two-way handshake*** ) . Đầu tiên , Client sẽ gửi username và password cho Server xác thực . Server sẽ tiến hành kiểm tra , nếu thành công thì sẽ thiết lập kết nối ; ngược lại sẽ không thiết lập kết nối với Client .
- Password được gửi dưới dạng không mã hóa ( ***ClearText*** ) và username / password được gửi đi kiểm tra 1 lần khi thiết lập kết nối .

    <img src=https://i.imgur.com/ycygf93.png>

#### **CHAP - Challenge Handshake Authentication Protocol**
- **CHAP** sử dụng kỹ thuật bắt tay 3 bước ( ***three-way handshake*** ) . **CHAP** được thực hiện ở lúc bắt đầu kết nối và luôn được lặp lại trong suốt quá trình kết nối được duy trì .
- Client muốn thiết lập kết nối tới Server , Server sẽ gửi đi 1 thông điệp có tên là ***challenge*** yêu cầu Client gửi giá trị để Server xác thực . Thông điệp gửi từ Server có chứa 1 `số ngẫu nhiên` dùng làm đầu vào cho thuật toán " ***hash*** " .
- Client nhận được thông điệp yêu cầu từ Server . Nó sẽ sử dụng thuật toán " ***hash*** " với đầu vào là `hostname` , `password` và `số ngẫu nhiên` vừa nhận được và tính toán ra 1 giá trị nào đó và gửi lại giá trị này cho Server .
- Server sẽ kiểm tra danh sách `username` ( nếu yêu cầu cấu hình `username` ) để tìm ra `username` nào giống với `hostname` của Client . Sau khi tìm được `username` đó , nó dùng thuật toán " ***hash*** " để mã hóa `password` tương ứng và số ngẫu nhiên trong thông điệp " ***challenge*** " ban đầu mà nó gửi cho Client để tính ra 1 giá trị nào đó . Và giá trị này sẽ so sánh với giá trị do Client gửi qua , nếu giống nhau thì xác thực thành công , nếu không thì kết nối sẽ bị xóa ngay .
- Ý tưởng khi cấu hình **CHAP** : mỗi đầu nối phải khai báo `username` và `password` . `Username` của R1 phải là `hostname` của R2 và `username` của R2 phải là `hostname` của R1 , `password` phải giống nhau .

    <img src=https://i.imgur.com/2Cu82FZ.png>

### **Cấu hình PPP**
- Bật cấu hình **PPP** :
    ```
    Router(config) # interface [name]
    Router(config-if) # encapsulation ppp
    ```
#### **Cấu hình chứng thực PAP**
- **B1 :** Tạo `username` và `password` trên Server
    ```
    Router(config) # username [username] password [password]
    ```
- **B2 :** : Enable **PPP** :
    ```
    Router(config-if) # encapsulation ppp
    ```
- **B3 :** : Cấu hình xác thực ( trên Server ) :
    ```
    Router(config-if) # ppp authentication pap
    ```
- **B4 :** : Enable PPP trên interface của Client :
    ```
    Router(config-if) # ppp pap sent-username [username] password [password]
    ```
#### **Cấu hình chứng thực CHAP**
<img src=https://i.imgur.com/qpdAA9i.png>

- **TH1 :** Các Router dùng `hostname` để chứng thực :
    ```
    R1(config) # username R2 password cisco
    R1(config) # interface s0/0
    R1(config-if) # encapsulation ppp

    R2(config) # username R1 password cisco
    R2(config) # interface s0/0
    R2(config-if) # encapsulation ppp
    R2(config-if) # ppp authentication chap
    ```
- **TH2 :** : Các Router gửi `username` và `password` bất kỳ :
    ```
    R1(config) # interface s0/0
    R1(config-if) # encapsulation ppp
    R1(config-if) # ppp chap hostname abc
    R1(config-if) # ppp chap password cisco

    R2(config) # interface s0/0
    R2(config-if) # encapsulation ppp
    R2(config-if) # ppp authentication chap
    ```
- Kiểm tra cấu hình :
    ```
    R# debug ppp authentication
    ```

