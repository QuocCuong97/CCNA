# Etherchannel
## **1) Mục đích ra đời**
- Để tăng băng thông đấu nối giữa các Switch , ta có thể sử dụng nhiều đường nối song song giữa các Switch .
- Nhược điểm :
    - Tốn kinh phí
    - Bị khóa 1 link theo giao thức **STP**

        <img src=https://i.imgur.com/Au2dCO4.png>

    => Giải pháp **Etherchannel** : một Switch sẽ thực hiện "bó" nhiều đường đấu nối thành 1 đường duy nhất , coi các đường này chỉ như 1 đường .
## **2) Đặc điểm**

<img src=https://i.imgur.com/4tlF4eN.png>

- Mỗi cổng **Etherchannel** bó được tối đa `8` cổng vật lý cùng loại .
- Các cổng phải được thống nhất về :

    - **Trunking Mode** : đều phải cùng các chuẩn trunking ( **dot1q** hay **ISL** ) , cùng mode trunking ( **on** / **desirable** / **auto** ) hoặc nếu không , đều phải ở cùng mode **Access** .

    - **Cấu hình VLAN** : nếu là cổng **trunk** , các cổng thành phần của 1 **Etherchannel** phải cho qua 1 danh sách **VLAN** giống nhau và phải cùng thống nhất với nhau về **Native VLAN** được sử dụng . Nếu là cổng **access** , các cổng thành phần phải cùng 1 **VLAN** .
    - **Speed** và **Duplex** : các cổng thành phần được yêu cầu phải thống nhất với nhau về **Speed** và **Duplex** trên cổng .
    - Thống nhất về các thiết lập **Spanning-Tree** trên cổng : **port cost** , **port-priority** ,...
- Phân phối tải :
    - Tải của **Etherchannel** sẽ không được phân phối đều trên các link thành phần mà được phân phối dựa vào việc xem xét các bit cuối cùng bên phải của địa chỉ MAC , địa chỉ IP , hay port của Segment TCP hay UDP .
    
    - Công nghê **Etherchannel** còn cho phép , với mỗi tieu chí vừa nêu ở trên , có thể kết hợp cả 2 thông số ***source*** à ***destination*** để ra quyết định phân phối tải . Khi sử dụng cả 2 thông số , các bit bên phải ngoài cùng của chúng được `XOR` và kết quả `XOR` này sẽ được sử dụng để ra quyết định .
    - Phép toán `XOR` `2 bit` nhị phân :
        ```
        Xét 2 nhị phân a và b : a XOR b = | 0 , nếu a = b
                                          | 1 , nếu a != b
        ```
## **3) Các giao thức thiết lập Etherchannel**
- Có 2 giao thức được sử dụng để thương lượng **Etherchannel** trên các Switch Cisco :
    - **PAgP - Port Aggregation Protocol** : là giao thức chuẩn của Cisco , chỉ chạy trên các Switch Cisco .
    - **LACP - Link Aggregation Control Protocol** : là giao thức chuẩn quốc tế của IEEE , được chuẩn hóa bởi ***IEEE 802.3ad*** .
- Điều kiện để thương lượng giao thức thành công và **Etherchannel** được thiết lập :
    - Các cổng thành phần của 1 kênh **Etherchannel** phải đồng nhất về các thông số **trunking** , **VLAN** , **Spanning-tree** ,...
    - Các cổng thành phần phải nối chung đến 1 Switch đầu xa . Trong quá trình trao đổi thông tin thương lượng , các Switch sẽ gửi cho nhau **device-id** . Chỉ những cổng nào nhận được cùng 1 **device-id** của lsang giềng mới có thể cùng nhau xây dựng 1 **Etherchannel** .
    - Phải chọn đúng mode hoạt động : **Chủ động** hay **Bị động**
        - Nguyên tắc : phía hoạt động ở mode **Chủ động** sẽ chủ động thương lượng đưa đầu kia thiết lập **Etherchannel** , phía hoạt động ở mode **Bị động** sẽ thụ động chờ đợi để phía kia thương lượng với mình .
        - Để một **Etherchannel** có thể được thiết lập , ít nhất 1 trong 2 đầu phải ở mode **Chủ động** .
        - **Chủ động - Chủ động** , **Chủ động - Bị động** sẽ thiết lập được **Etherchannel** ; **Bị động - Bị động** sẽ không thể thiết lập .
        - Với **PAgP** :
            - **Chủ động** = ***Desirable***
            - **Bị động** = ***Auto***
        - Với **LACP**
            - **Chủ động** = ***Active***
            - **Bị động** = ***Passive***

                <img src=https://i.imgur.com/86N2Vc0.jpg>

## **4) Cấu hình Etherchannel**

<img src=https://i.imgur.com/l4rx7Oh.png>

### **4.1) Cấu hình tĩnh**
- Đi vào mode cấu hình của các cổng thành phần và dùng lệnh :
    ```
    Switch(config-if) # channel-group [number] mode on
    ```
   **VD :** Trên SW1 , SW2 :
   ```
   SW1(config) # interface range f0/1-2
   SW1(config-if) # channel-group 12 mode on

   SW2(config) # interface range f0/1-2
   SW2(config-if) # channel-group 12 mode on
   ```
- Kiểm tra cấu hình :
    ```
    Switch# show etherchannel summary
    ```
### **4.2) Cấu hình bằng các giao thức**
- Cấu hình **Etherchannel** với **PAgP** :
    ```
    Switch(config-if) # channel-protocol pagp
    Switch(config-if) # channel-group [number] mode [desirable|auto]
    ```
- Cấu hình **Etherchannel** với **LACP** :
    ```
    Switch(config-if) # channel-protocol lacp
    Switch(config-if) # channel-group [number] mode [active|passive]
    ```


