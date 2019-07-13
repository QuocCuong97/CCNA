# Dynamic Routing
## **1) Khái niệm**
- **Định tuyến động ( Dynamic Routing )** : các Router tự trao đổi thông tin về các địa chỉ mạng trên sơ đồ , tự chạy một phương thức tính toán nào đó để xác định xem để đi đến các mạng này thì phải sử dụng đường đi nào là tối ưu .
- Với phương thức định tuyến động , các Router cần phải chạy các ***Giao thức định tuyến ( Routing Protocol )*** để có thể tương tác trao đổi thông tin và tính toán định tuyến .
## **2) Phân loại các giao thức định tuyến**

<img src=https://i.imgur.com/k2Qfjbm.jpg>

### **2.1) EGP và IGP**
- **Giao thức định tuyến ngoài ( EGP - Exterior Gateway Protocol )** tiêu biểu là giao thức **BGP ( Border Gateway Protocol )** là loại giao thức được dùng để chạy giữa các Router thuộc **AS - Anonymous System** ( vùng tự trị ) khác nhau , phục vụ cho việc trao đổi thông tin định tuyến . Các **AS** thường là các **ISP** . Như vậy , định tuyến ngoài thường được dùng cho mạng Internet toàn cầu để trao đổi số lượng lớn thông tin định tuyến rất lớn giữa các **ISP** với nhau .

- **Giao thức định tuyến trong ( IGP - Interior Gateway Protocol )** gồm các giao thức **RIP** , **OSPF** , **EIGRP** . **IGP** là loại giao thức chạy giữa các Router nằm bên trong 1 **AS** .
### **2.2) Distance-vector , Link-state và Hybrid**
- **Distance-vector** : Mỗi Router sẽ gửi cho láng giềng của nó toàn bộ bảng định tuyến của nó theo định kỳ . Giao thức tiêu biểu của hình thức này là **RIP** . Đặc thù của loại hình định tuyến này là có khả năng bị loop nên cần 1 bộ quy tắc chống loop có thể sẽ làm chậm tốc độ hội tụ của giao thức

- **Link-state** : Mỗi Route sẽ gửi các bản tin trạng thái đường link **LSA** cho các Router khác . Việc tính toán định tuyến được thực hiện bằng giải thuật **Dijkstra** .
- **Hybrid** : Tiêu biểu là giao thức **EIGRP** của Cisco . Loại giao thức này kết hợp các đặc điểm của 2 loại trên . Tuy nhiên , thực chất thì **EIGRP** vẫn là giao thức loại **Distance-vector** nhưng đã được cải tiến thêm để tăng tốc độ hộ tụ và quy mô hoạt động nên còn được gọi là **Advanced Distance-vector** .
### **2.3) Classful và Classless**
- Các giao thức **Classful** : ROuter sẽ không gửi kèm theo `subnet-mask` trong bảng định tuyến của mình . Từ đó các giao thức **Classful** không hỗ trợ các sơ đồ **VLSM** và mạng gián đoạn ( ***discontiguos network*** ) . Giao thức tiêu biểu là **RIPv1** ( trước đây có thêm cả **IGRP** nhưng hiện giờ giao thức này đã được gỡ bỏ trên các IOS mới của Cisco ) .

- Các giao thức **Classless** : Ngược lại với **Classful** , Router có gửi kèm theo `subnet-mask` trong bản tin định tuyến . Từ đó các giao thức **classless** có hỗ trợ các sơ đồ **VLSM** và mạng gián đoạn ( ***discontiguos network*** ) . Các giao thức **Classless** : **RIPv2** , **OSPF** , **EIGRP** .
## **3) Giá trị AD**
- **AD - Administrative Distance** là giá trị được sử dụng để đo đạc mức độ ưu tiên giữa các kỹ thuật định tuyến . Khi một Router học được những đường đi khác nhau từ nhiều phương thức định tuyến khác nhau cho cùng 1 địa chỉ đích , Router sẽ chọn đường đi theo phương thức nào có **AD** nhỏ nhất .
- Bảng các giá trị **AD** :

    | Kỹ thuật định tuyến | Giá trị AD |
    |---------------------|------------|
    | Connected | 0 |
    | Static | 1 ( hoặc 0) |
    | EIGRP | 90 |
    | OSPF | 110 |
    | RIP | 120 |
## **4) Giá trị Metric**
- **RIP** : `hop count`
- **OSPF** : `cost` ( dựa vào `bandwidth` )
- **EIGRP** : `bandwidth` , `delay` , `load` , `reliability` , `MTU`
## **5) Tốc độ hội tụ của các giao thức**
- **Static Route** : **không hội tụ** với mọi thay đổi diễn ra trên mạng ngoại trừ các chuyển đổi trạng thái ***up/down*** của ác cổng chính Router được cấu hình **static route** .
- **RIP** : hội tụ **chậm** .
- **OSPF** : hội tụ **nhanh** .
- **EIGRP** : hội tụ **rất nhanh** .
## **6) Bộ tham số định thời của các giao thức**
- **RIP**
    - **Update Timer** : `30s`
    - **Holddown-Timer** : `180s`
    - **Invalid-Timer** : `180s`
    - **Flush Timer** : `240s`
- **OSPF**
    - **Hello-Timer** : `10s`
    - **Dead-Timer** : `40s`
- **EIGRP**
    - **Hello-Timer** : `5s`
    - **Hold-Timer ( Dead-Timer )** : `15s`

