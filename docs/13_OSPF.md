# OSPF - Open Shortest Path First ( O )
## **1) Đặc điểm**
- Là 1 giao thức Link-state điển hình . Mỗi Router khi chạy giao thức sẽ gửi bản tin **LSA ( *Link-State Advertisement* )** của nó cho tất cả các Router trong vùng ( **area** ) . Sau 1 thời gian trao đổi , các Router sẽ đồng nhất bảng cơ sở dữ liệu **LSDB ( *Link-State Database* )** với nhau , mỗi Router đều có được "bản đồ mạng" của cả vùng . Từ đó mỗi Router sẽ chạy giải thuật ***Dijkstra*** tính toán ra 1 cây đường đi ngắn nhất ( ***Shortest Path Tree*** ) và dựa vào cây này để xây dựng bảng định tuyến .
- **OSPF** có **AD** = `110`
- **Metric** = `(10^8)\bandwidth`
- **OSPF** chạy trực tiếp trên nền IP , có **protocol-id** là `89`
- Là giao thức chuẩn quốc tế , được định nghĩa trong ***RFC-2328***
- Là giao thức loại **classless** ( hỗ trợ **VLSM** và **CIDR** )
- Tốc độ hội tụ nhanh
- Hỗ trợ xác thực
- Khả năng mở rộng tốt , được khuyến nghị dùng cho môi trường mạng có quy mô lớn
- Sử dụng loại địa chỉ **Multicast** `224.0.0.5` và `224.0.0.6`
## **2) Hoạt động của OSPF**
- **B1** : Bầu chọn Router-id
- **B2** : Thiết lập quan hệ láng giềng
- **B3** : Trao đổi trạng thái đường link **LSA**
- **B4** : Tính toán xây dựng bảng định tuyến
## **3) Các thuật ngữ cơ bản**
- **Router-ID** :
    - Đầu tiên , khi 1 Router chạy **OSPF** , nó phải xây dựng một giá trị dùng để định danh duy nhất cho nó trong các Router chạy **OSPF** . Giá trị này được gọi là **Router-ID** .
    - **Router-ID** của Router chạy **OSPF** có định dạng 1 địa chỉ IP . Mặc định , tiến trình **OSPF** trên mỗi Router sẽ tự động bầu chọn **Router-ID** là địa chỉ IP cao nhất trong các interface đang ***active ( up/up )*** , ưu tiên cổng `Loopback ` .
- **Area-ID** : 
    - Để giảm tải bộ nhớ cũng như tải tính toán cho mỗi Router và giảm thiểu lượng thông tin định tuyến cần trao đổi , các Router chạy **OSPF** được chia thành nhiều vùng ( **area** ) , mỗi Router lúc này chỉ phải ghi nhớ thông tin cho 1 vùng mà nó ở trong đó .
    - Mỗi vùng sẽ có giá trị định danh là **Area-ID** . **Area-ID** có thể được hiển thị dưới dạng 1 số tự nhiên hoặc dưới dạng 1 địa chỉ IP . **VD :** Vùng 1 có thể được hiển thị là "`area 1`" hoặc "`area 0.0.0.1`" .
    - Một nguyên tắc bắt buộc trong phân vùng **OSPF** là nếu chia thành nhiều vùng thì bắt buộc phải tồn tại 1 vùng mang số hiệu `0` - `Area 0` . `Area 0` còn được gọi là **Backbone Area** và mọi vùng khác bắt buộc phải có kết nối về `area 0` .

        <img src=https://i.imgur.com/OTS5urs.png>

    - Trong thao tác cấu hình , **Area-ID** được gán cho link của Router chứ không phải gán cho bản thân Router . Về bản chất , với **OSPF** , không có khái niệm Router thuộc về 1 **area** và chỉ có khái niệm link của Router thuộc về **area** nào .
    - Những Router mà có tất cả các link đều được gắn vào 1 **area** thì sẽ lọt hẳn vào **area** đó và được gọi là các **Internal Router** ( VD như R4 của area 1 ) , các **Internal Router** chỉ việc ghi nhớ trạng thái đường link của Router thuộc về vùng mà nó nằm bên trong . Những Router có các link thuộc về các vùng khác nhau được gọi là các **ABR Router** ( **Area Border Router** ) ( VD như R2 và R3) .

- **Hello Timer** và **Dead Timer**
    - **Hello Timer** là khoảng thời gian định kỳ Router gửi gói tin ***Hello*** ra khỏi 1 cổng chạy **OSPF** . Khi một Router nhận được bản tin ***Hello*** từ láng giềng , nó sẽ khởi động **Dead Timer** . Nếu sau khoảng thời gian **Dead Timer** mà Router không nhận được gói tin ***Hello*** từ láng giềng , nó sẽ coi như láng giềng này sẽ không còn và sẽ xóa mọi thông tin mà nó học được từ láng giềng . Ngược lại , cứ mỗi lần nhận được gói tin ***Hello*** từ láng giềng , **Dead Timer** lại được reset .

    - Giá trị mặc định của **Dead Timer** là `40s` và **Hello Timer** là `10s` .
- **ASBR Router** : là những Router thực hiện tiến trình redistribute những mạng bên ngoài vùng **OSPF** vào vùng thông qua **External LSA** .
- **Backbone Router** : là những Router có ít nhất 1 cổng tham gia vào `area 0` .
- **LSDB - Link-State Database** : bảng cơ sở dữ liệu trạng thái đường link là 1 bảng trên Router ghi nhớ mọi trạng thái đường link của mọi Router trong vùng .
- **LSA - Link-State Advertisement** : các Router sẽ không trao đổi với nhau cả 1 bảng **LSDB** mà sẽ trao đổi với  nhau từng đơn vị thông tn gọi là **LSA** .
- **LSU - Link-State Update** : là 1 gói tin chứa đựng bản tin **LSA** mà các Router thực sự trao đổi với nhau .
> ## **4) Các môi trường OSPF**
- ### **Serial Point-to-Point**

    <img src=https://i.imgur.com/1xg08uC.png>

    - Hai Router láng giềng sẽ ngay lập tức trao đổi thông tin cho nhau qua kết nói Point-to-Point và chuyển trạng thái quan hệ từ 2-WAY sang mức độ **FULL** . Quan hệ **FULL** giữa 2 Router kết nối qua Serial Pont-to-Point được ký hiệu là "`FULL/- `"
- ### **Broadcast MultiAccess**
    - Là môi trường Ethernet LAN .
    - Trong mỗi MultiAccess , một **DR** Router sẽ được bầu ra . Một Router khác đóng vai trò là **Backup DR (BDR)** để đề phòng **DR** bị ***down*** . Các Router còn lại gọi là **DROther** . Các Router **DROther** khi trao đổi thông tin định tuyến sẽ không gửi trực tiếp cho nhau mà sẽ gửi lên cho **DR** và **BDR** . Sau đó **DR** này sẽ forward gói tin xuống cho các **DROther** .
    - Thông tin **OSPF** được gửi đến **DR / BDR** theo địa chỉ `224.0.0.6` và được **DR** chuyển xuống **DROther** theo địa chỉ `224.0.0.5` .
## **5) Quá trình bầu chọn DR và BDR**
- **DR - Designated Router**
- **BDR - Backup Designated Router**
- **DROther** - Các Router còn lại chạy **OSPF**
- Quá trình bầu chọn chú ý đến 2 tham số : **độ ưu tiên ( priority )** và **Router-ID** :
    - Tham số **priority** được chọn trước tiên , giá trị **priority** nằm trong khoảng từ `0` đến `255` .
    - Nếu **priority** đặt là `0` thì Router này sẽ không tham gia vào quá trình bình chọn **DR** và **BDR**
    - Router nào có **priority** cao nhất sẽ được chọn là **DR** , cao thứ 2 là **BDR** .
    - Mặc định , giá trị **priority** trong OSPF là `1` .
    - Khi giá trị bằng nhau thì Router sẽ bầu chọn **DR** dựa vào tham số thứ 2 là **Router-ID** .
    - Trong hệ thống mạng dùng **OSPF** không cấu hình cổng Interface `Loopback` thì giá trị **Router-ID** được chọn là địa chỉ IP lớn nhất của các interface ***active*** . Nếu có cổng `Loopback` thì cổng `Loopback` được chọn , trường hợp có nhiều cổng `Loopback` thì chọn cổng `Loopback` có IP cao nhất .
- Luật bầu chọn **DR** : khi 1 **DR** đã được bầu chọn xong , nếu Router mới tham gia vào môi trường **MultiAccess** có **priority** hay **Router-ID** cao hơn Router **DR** cũng không thể chiếm quyền **DR** hiện tại . Chỉ khi nào **DR** hiện tại ***down*** , các Router khác mới có quyền tranh quyền **DR** .
- Quan hệ giữa các cặp Router
    - Các **DROther** không bao giờ trao đổi trạng thái đường link với nhau nên mối quan hệ giữa chúng chỉ dừng ở mức độ 2-WAY .
    - Các **DROther** có trao đổi dữ liệu với **DR** và **BDR** => quan hệ láng giềng với các Router **DR** và **BDR** sẽ là **FULL**
## **6) Tính toán Metric với OSPF**
- **Metric** trong **OSPF** là `cost` tích lũy trên đường đi đến mạng đích . Giá trị `cost` trên mỗi cổng được xác định dựa vào `bandwidth` trên cổng theo công thức
```
                Metric = (10^8) / bandwidth(bps)
```
| Port | Default Bandwidth (kbps) | Cost |
|------|--------------------------|------|
| Ethernet | 10000 | 10 |
| Fast Ethernet | 100000 | 1 |
| Gigabit Ethernet | 1000000 | 0.1 |
| Serial | 1544 | 64 |
| Loopback | 10000 | 10 |
## **7) Cấu trúc lệnh OSPF**
```
Router(config) # router ospf [process-id]
Router(config-router) # network [connected network] [wildcard mask] area [area-id]
```
- Trong đó
    - **Process-ID** : chỉ số tiến trình của **OSPF** , mang tính chất cục bộ , có giá trị từ `1` đến `65536` 
    - **Connected network** : dải mạng tham gia **OSPF**
    - **Wildcard-mask** : điều kiện để kiểm tra giữa địa chỉ cấu hình trong address và địa chỉ giữa các cổng trên Router
    - **Area-ID** : vùng mà cổng tương ứng thuộc về kiến trúc **OSPF**
- Hiệu chỉnh **Router-ID** :
    ```
    Router(config-router) # router-id A.B.C.D
    ```
    Sau đó 
    ```
    Router # clear ip ospf process => Yes           ( khởi động lại quá trình OSPF )
    ```
- Hiệu chỉnh **Hello Timer** và **Dead Timer** :
    ```
    Router(config-if) # ip ospf | hello-interval | [seconds]
                                |  dead-interval |
    ```
- Hiệu chỉnh `bandwidth` của cổng :
    ```
    Router(config-if) # bandwidth [number]        ( đơn vị là kbps )
    ```
- Hiệu chỉnh `cost` :
    ```
    Router(config-if) # ip ospf cost [value]
    ```
- Các lệnh kiểm tra cấu hình **OSPF**
    ```
    Router # show ip protocol
    Router # show ip route
    Router # show ip ospf interface
    Router # show ip ospf neighbor
    Router # debug ip ospf events
    Router # debug ip ospf packet
    ```
- Chứng thực trong **OSPF**
    - Dạng ***ClearText***
    ```
    Router(config) # interface f0/0
    Router(config-if) # ip ospf authentication
    Router(config-if) # ip ospf authentication-key [password]
    ```
    - Dạng ***MD5***
    ```
    Router(config) # interface f0/0
    Router(config-if) # ip ospf authentication message-digest
    Router(config-if) # ip ospf message-digest-key 1 md5 [password]
    ```





