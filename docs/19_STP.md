# STP - Spanning Tree Protocol
## **1) Các lỗi có thể xảy ra khi triển khai dự phòng Switch**
- **Bão Broadcast ( Broadcast Storm )**

    <img src=https://i.imgur.com/xCAS8Av.png>

- **Trùng lặp Frame (  Duplicate Unicast Frames )**

    <img src=https://i.imgur.com/wELRiAu.png>

- **Bảng MAC không ổn định ( MAC database instability )** 

    <img src=https://i.imgur.com/9JZkWji.png>

## **2) Tiến trình STP**
- **B1 :** Bầu chọn **Root Switch**
    - Đầu tiên , các Switch sẽ gửi các gói tin **BPDU - Bridge Protocol Data Unit** ra khỏi cổng của mình ( `2s/lần` ). **BPDU** chứa nhiều thông tin quan trọng của **STP** , trong đó 1 giá trị quan trọng được trao đổi là **Bridge-ID** . **Bridge-ID** là giá trị để định danh cho Switch trong **STP** , dài `8 byte` và được cấu tạo từ :
        - **Priority** ( dài `2 byte` ) : `0` <= **priority** <= 65535 , mặc định là `32768`
        - Địa chỉ MAC của Switch ( dài `6 byte` )
    - Quy tắc bầu chọn **Root Switch** :
        - Switch nào có **priority** nhỏ nhất sẽ làm **Root Switch** .
        - Nếu tồn tại nhiều Switch có **priority** bằng nhau , Switch có MAC nhỏ nhất sẽ làm Root Switch .
- **B2 :** Bầu chọn các **Root Port** trên mỗi **Non-Root Switch**
    - **Tổng cost tích lũy** :
        - **Root Port** là port cung cấp đường về cho Switch đang xét mà có tổng cost nhỏ nhất

            | Bandwidth | Cost |
            |-----------|------|
            | `10 Mbps` | 100 |
            | `100 Mbps` | 19 |
            | `1 Gbps` | 4 |
            | `10 Gbps` | 2 |

        - Để xác định được **tổng cost tích lũy** từ 1 cổng đến **Root Switch** , thực hiện tính ngược từ **Root Switch** về theo qui tắc : mỗi khi đi vào 1 cổng nào thì cộng dồn **cost** của cổng ấy , đi ra khỏi cổng thì không cộng .
    - **Sender-Bridge ID** :
        - Khi **tổng cost tích lũy** ở các cổng của **Root Port** bằng nhau , phải sử dụng yếu tố **Sender-Bridge ID** .
        - **Sender-Bridge ID** là **Bridge-ID** của Switch láng giềng đến cổng đang xét . Nếu tồn tại 2 cổng có **cost** bằng nhau , cổng nào nối đến láng giềng có **Bridge-ID** nhỏ hơn sẽ được bầu chọn là **Root Port** .
    - **Port-ID** :
        - Là giá trị được sử dụng để định danh cho mỗi cổng Ethernet LAN trên Switch .
        - Giá trị **Port-ID** gồm `2 byte` gồm 2 thành phần : **port-priority** ( `1 byte `) và **port-number** ( `1 byte` ) . **Port-priority** nhận giá trị từ `0` đến `255` , default là `128` ; **port-number** chính là số thứ tự vật lý của cổng trên phần cứng của Switch .
        - Trong trường hợp 2 cổng có cùng **cost** và **Sender-Bridge ID** , **Port ID** được sử dụng để xác định **Root Port** : cổng nào đấu nối đến cổng đầu xa có **Port-ID** nhỏ hơn , cổng ấy sẽ được bầu chọn làm **Root Port** . Việc so sánh **Port-ID** được tiến hành trước hết với **port-priority** : **port-priority** nhỏ hơn được coi là nhỏ hơn , nếu **port-priority** bằng nhau thì mới so sánh **port-number** .
        
- **B3 :** Bầu chọn các **Designated Port** :
    - **Designated Port** là port cung cấp đường về **Root Switch** cho phân đoạn mạng đang xét mà có **tổng cost tích lũy** nhỏ nhất . Các nhân tố được sử dụng khi xem xét **cost tích lũy** cũng là **Sender-Bridge ID** và **Port-ID** .
    - 1 link chỉ được quyền có 1 **Designated Port** .
    - Quy tắc sử dụng để xác định **Designated Port**
        - Tất cả các cổng của **Root Switch** đều là **Designated Port**
        - Trên link point-to-point , nếu đầu kia là **Root Port** , đầu nàu sẽ là **Designated Port** .
        - Nếu xảy ra trường hợp có 1 link có 2 cổng cung cấp đường về **Root Switch** có **cost tích lũy** bằng nhau , **Sender-Bridge ID** và **Port-ID** được dùng để xác định **Designated Port** .
- **B4 :** Khóa các port còn lại :
    - Tại bước này , port nào trên Switch chưa có vai trò cụ thể sẽ bị khóa và trở thành **Non-designated Port** .
## **3) Các bộ định thời trong STP**
- **Hello-Timer** : Có giá trị mặc định là `2s` , là khoảng thời gian định kỳ cho **Root Switch** gửi **BPDU** ra khỏi cổng của nó .
- **Forward Delay Timer** : Có giá trị là `15s` , là thời gian 1 cổng chạy **STP** phải ở trạng thái ***Listening*** , ***Learning*** trước khi đi vào ***Forwarding*** trong quá trình hội tụ .
- **Max-Age Timer** : Có giá trị mặc định là `20s` . Khi cổng đang nhận **BPDU** ( cổng Root hoặc cổng khóa ) bỗng nhiên nhận được **BPDU** kém hơn **BPDU** nó đang tiếp nhận ( **inferior BPDU** ) ,  cổng sẽ chờ hết thời gian **max-age timer** rồi thực hiện các hoạt động hội tụ .
    - ***Lưu ý*** : **BPDU** được so sánh theo trình tự **Root Bridge ID** -> **Tổng cost tích lũy** -> **Sender-Bridge ID** -> **Port-ID**
## **4) Các trạng thái cổng trong STP**
- ***Disabled*** : cổng tham gia **STP** đang không ***active*** ( không ***up/up*** )
- ***Blocking*** : đây là trạng thái khi cổng bị khóa . Ở trạng thái này , cổng chỉ tiếp nhận **BPDU** , không học địa chỉ MAC vào bảng MAC và không forward dữ liệu .
- ***Listening*** : ở trạng thái này , cổng có thể tiếp nhận hoặc gửi **BPDU** , cổng không học địa chỉ MAC vào bảng MAC và không forward dữ liệu .
- ***Learning*** : ở trạng thái này , cổng có thể nhận hoặc gửi **BPDU** , cổng có thể học địa chỉ MAC vào bảng MAC nhưng không forward dữ liệu .
- ***Forwarding*** : cổng ở trạng thái này có thể nhận hoặc gửi **BPDU** , có thể học địa chỉ MAC vào bảng MAC và có thể forward dữ liệu .
- Khi chạy **STP** trên Switch , nếu 1 cổng nào được tính toán chỉ ra cần phải khóa , cổng sẽ đi vào trạng thái ***Blocking*** và bị khóa ngay lập tức . Tuy nhiên nếu 1 cổng được tính toán là **Root Port** hay **Designated Port** , cổng sẽ chưa được forward dữ liệu ngay mà phải đi vào trạng thái ***Listening*** và ở đó 1 thời gian **Forward Delay Timer** ( `15s` ) rồi mới chuyển qua ***Forwarding***. Ở trạng thái ***Forwarding*** ,mới có thể forward dữ liệu . Như vậy , trong điều kiện bình thường , một cổng Switch tham gia **STP** nếu không bị khóa phải mất `30s` mới có thể sử dụng được . Các trạng thái ***Learning*** và ***Listening*** đưa ra để ngăn chặn Loop . Tuy nhiên , việc sử dụng các trạng thái này đã dẫn đến **STP** hội tụ quá lâu ( `30s` ) .

    **=>** Để khắc phục , Cisco đưa ra các tính năng **Portfast** , **Uplinkfast** , **Backbonefast** ; IEEE đã đưa ra 1 version khác của **STP** cho phép khắc phục hội tụ chậm là **Rapid STP** ( ***IEEE 802.1w*** ) .
## **5) Cisco Per-VLAN STP ( pVST+ )**
- **STP** ( ***802.1D*** ) truyền thống còn được gọi là **Common STP** ( **CST** ) , không xem xét vấn đề chia **VLAN** , chạy 1 tiến trình duy nhất cho tất cả các **VLAN** => lãng phí việc sử dụng đường truyền .
- Cisco khắc phục điều này bằng cách đưa ra kỹ thuật **pVST+** trên các dòng sản phẩm Switch Catalyst của mình .
- Ý tưởng của kỹ thuật này là mỗi **VLAN** sẽ chạy 1 **STP** riêng 
=> Nếu hiệu chỉnh thích hợp , có thể thực hiện cân bằng tải giữa các **VLAN** , từ đó sử dụng đường truyền tối ưu hơn nhiều .
- Với **pVST+**, **VLAN-ID** của các **VLAN** sẽ được nhúng vào trong các gói tin **BPDU** để phân biệt **BPDU** của các tiến trình **STP** của các **VLAN** khác nhau . Giá trị **VLAN-ID** dài `12 bit` sẽ được cài vào **priority** của **Bridge-ID** trên các **BPDU** . **Priority** dài `16bit` nên chỉ còn `4 bit` cho **priority**, `12 bit` còn lại cho **VLAN-ID** => **Priority** với **pVST+** luôn phải là bội số của `4096` ( = `2`<sup>`12`</sup> )
## **6) Cấu hình STP**
- Bật | Tắt **STP** trên các **VLAN** :
    ```
    Switch(config) # [no] spanning-tree vlan [vlan-id]
    ```
    Mặc định , **STP** được bật trên mọi **VLAN** .

- Cấu hình Switch trở thành **Root Switch** :
    ```
    Switch(config) # spanning-tree vlan [vlan-id] root [primary|secondary]
    ```
    - **Primary** : Switch sẽ trở thành **Root Switch** cho **VLAN**
    - **Secondary** : Switch sẽ trở thành **Root Switch** dự phòng cho **VLAN**
- Hiệu chỉnh **priority** cho Switch :
    ```
    Switch(config) # spanning-tree vlan [vlan-id] priority [priority]
    ```
    - **Priority** = `n.4096`
- Hiệu chỉnh **cost** trên cổng :
    ```
    Switch(config-if) # spanning-tree vlan [vlan-id] cost [cost]
    ```
- Hiệu chỉnh các **Port-priority** trên cổng :
    ```
    Switch(config) # spanning-tree vlan [vlan-id] port-priority [priority]
    ```
    - **Priority** = `n.16`
- Hiệu chỉnh các **Timer** của **STP** :
    ```
    Switch(config) # spanning-tree vlan [vlan-id] | hello-timer   | [seconds]
                                                  | forward-timer |
                                                  |    max-age    |
    ```
-  Chọn mode **STP** trên Switch :
    ```
    Switch(config) # spanning-tree mode [pvst|rapid-pvst|mst]
    ```
    - Mặc định , Switch Cisco hoạt động ở chế độ **pVST+** ( **pvst** ) .
- Kiểm tra **spanning-tree** :
    ```
    Switch# show spanning-tree [vlan[vlan-id]]
    ```
    



