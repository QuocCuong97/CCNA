# EIGRP - Enhanced Interior Gateway Protocol ( D )
## **1) Đặc điểm**
- **EIGRP** là 1 giao thức định tuyến do Cisco phát triển , chỉ chạy trên các sản phẩm của Cisco
- Là giao thức định tuyến lai ( là 1 giao thức **Distance-Vector** nhưng lại có đặc điểm của giao thức **Link-State** )
- Sử dụng số IP **protocol-id** là `88`
- Sử dụng thuật toán ***DUAL*** - ***Diffusing Update Algorithym*** để tính toán đường đi và chống loop
- Hội tụ nhanh
- Hỗ trợ xác thực ***MD5***
- Hỗ trợ **VLSM**
- Theo mặc định , cân bằng tải với các đường có `metric` bằng nhau . Có thể cân bằng tải với các đường có `metric` không bằng nhau
- Chỉ số **AD** của **EIGRP** là `90` cho các Router *Internal* và là `170` cho các Router `External`
- Khả năng mở rộng cao , được sử dụng trong các mạng lớn
- Địa chỉ Multicast `224.0.0.10`
- Sử dụng 1 công thức tính toán `metric` rất phức tạp dựa trên nhiều thông số : `bandwidth` , `delay` , `load` , `reliability` , `MTU ` và bộ tham số `k` tương ứng
## **2) Hoạt động của EIGRP**
- **B1 :** Các Router phát hiện các láng giềng của nó , danh sách các láng giềng được lưu trữ trong bảng "**neighbor table**"
- **B2 :** Mỗi Router sẽ trao đổi các thông tin về cấu trúc mạng với các láng giềng của nó
- **B3 :** Router đặt những thông tin về cấu trúc mạng học được vào cơ sở dữ liệu về cấu trúc mạng ( **topology table** )
- **B4 :** Router chạy thuật toán ***DUAL*** với cơ sở dữ liệu đã thu thập được ở bước trên để tính toán , tìm ra đường đi tốt nhất đến mỗi một mạng trong cơ sở dữ liệu
- **B5 :** Router đặt các tuyến đường đi tốt nhất vào bảng định tuyến
## **3) Các thuật ngữ cơ bản**
- **Hello-Timer** : ngay khi bật **EIGRP** trên 1 cổng , Router sẽ gửi gói tin ***Hello*** ra khỏi cổng để thiết lập quan hệ láng giềng với Router kết nối trực tiếp với mình . Các gói tin được gửi đến địa chỉ **Multicast** dành riêng cho **EIGRP** là `224.0.0.10` với **Hello-Timer** là `5s` .
- **AS - Autonomous System** : các Router sẽ chỉ trao đổi thông tin **EIGRP** với các Router thuộc cùng **AS** với mình .
- **Holddown-Timer** ( Dead Timer ) : mặc định là `15s` .
- **FD - Feasible Distance** : với mỗi đường đi , giá trị metric từ Router đang xét đến mạng đích gọi là **FD** .
- **AD - Advertised Distance** : cũng với đường đi ấy , giá trị từ Router láng giềng đến mạng đích được gọi là **AD** .

    <img src=https://i.imgur.com/bPJFUP8.png>

- **Successor** : Trong tất cả các đường đi có cùng 1 đích đến , đường nào có **FD** nhỏ nhất , sẽ được bầu là **Successor** , Router láng giềng trên đường này được gọi là **Successor Router** . Đường **successor** sẽ được đưa vào bảng định tuyến để sử dụng chính thức làm đường đi đến đích . 
- **Feasible Successor** : Trong tất cả các đường còn lại có **FD** > **FD** của **Successor** , đường nào có **AD** < **FD** của **Successor** , đường đó sẽ được bầu chọn là **Feasible Successor** và được sử dụng làm dự phòng cho **Successor** .
    - Nếu không có đường nào thỏa mãn **Feasible Successor** : Trong trường hợp này , **Successor** vẫn được đưa vào bảng định tuyến để sử dụng làm đường đi chính thức nhưng không có đường backup . Khi đường này ***down*** , Router chạy **EIGRP** sẽ thực hiện 1 kỹ thuật gọi là **Query** : nó sẽ phát các gói tin truy vấn đến các láng giềng , hoạt động truy vấn sẽ tiếp tục được lan truyền cho đến khi tìm ra được đường đi về hoặc không còn đường nào về đích được nữa .
## **4) Tính toán Metric với EIGRP**
- `Metric` trong **EIGRP** được tính bằng công thức sau : 

    <img src=https://i.imgur.com/lliYMko.png>

- Trong đó : 
    - `Bandwitdh` ( băng thông ) đơn vị là `kbps`
    - `Delay` ( độ trễ ) đơn vị là `10 microsecond`
    - `Load` ( khả năng truyền tải ) , `Reliability` ( độ tin cậy ) là các đại lượng vô hướng không có đơn vị .
    - Giá trị `k` mặc định là `k1 = k3 = 1` ; `k2 = k4 = k5 = 0`
      
    => Công thức mặc định là :
    
    ```
                             Metric = Bandwidth + Delay  
    ```

    | Loại cổng | Bandwidth mặc định (kbps) | Delay mặc định (us) |
    |-----------|---------------------------|---------------------|
    | Ethernet | 10000 | 100 |
    | Fast Ethernet | 100000 | 10 |
    | Serial | 1544 | 2000 |
## **5) Cấu trúc lệnh**
- Kích hoạt giao thức **EIGRP** : 
    ```
    Router(config) # router eigrp [autonomous-system]
    Router(config) # network [network join EIGRP]
    Router(config) # no auto-summary
    ```
Trong đó **AS** có giá trị từ `1` -> `65535` , giá trị này phải giống nhau ở tất cả các Router chạy **EIGRP** .
- Kiểm tra cấu hình **EIGRP**
    ```
    Router # show ip eigrp neighbors
    Router # show ip eigrp topology
    Router # show ip route
    Router # show ip route eigrp
    Router # show ip protocols
    Router # show ip eigrp traffic
    ```
- Cân bằng tải trên những đường không đều nhau :
    ```
    Router(config) # router eigrp [AS]
    Router(config-router) # variance [number]
    ```
- Chứng thực trong **EIGRP** ( chỉ hỗ trợ ***MD5*** ) :
    ```
    Router(config) # key chain [keychain]
    Router(config-keychain) # key [key-id]
    Router(config-keychain-key) # key-string [password]
    Router(config) # interface [name]
    Router(config-if) # ip authentication mode eigrp [AS] md5
    Router(config-if) # ip authentication key-chain eigrp [AS] [keychain]
    ```

