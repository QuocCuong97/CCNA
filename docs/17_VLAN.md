# VLAN - Virtual LAN
## **1) Khái niệm VLAN**
- **VLAN (Virtual LAN)** là một hay nhiều LAN ảo được chia ra từ một Switch vật lý . Nói cách khác VLAN là chia một Switch vật lý thành nhiều Switch logic độc lập.

    <img src=https://i.imgur.com/lUrAgKK.jpg>

- Lợi ích của **VLAN** :
    - ***Tiết kiệm băng thông của hệ thống mạng***: VLAN chia mạng LAN thành nhiều đoạn (segment) nhỏ, mỗi đoạn đó là một vùng quảng bá (broadcast domain). Khi có gói tin quảng bá (broadcast), nó sẽ được truyền duy nhất trong VLAN tương ứng. Do đó việc chia VLAN giúp tiết kiệm băng thông của hệ thống mạng.
    - ***Tăng khả năng bảo mật***: Do các thiết bị ở các VLAN khác nhau không thể truy nhập vào nhau (trừ khi ta sử dụng router nối giữa các VLAN). Như trong ví dụ trên, các máy tính trong VLAN kế toán (Accounting) chỉ có thể liên lạc được với nhau. Máy ở VLAN kế toán không thể kết nối được với máy tính ở VLAN kỹ sư (Engineering).
    - ***Dễ dàng thêm hay bớt máy tính vào VLAN***: Việc thêm một máy tính vào VLAN rất đơn giản, chỉ cần cấu hình cổng cho máy đó vào VLAN mong muốn.
    - ***Giúp mạng có tính linh động cao***: VLAN có thể dễ dàng di chuyển các thiết bị. Giả sử trong ví dụ trên, sau một thời gian sử dụng công ty quyết định để mỗi bộ phận ở một tầng riêng biệt. Với VLAN, ta chỉ cần cấu hình lại các cổng switch rồi đặt chúng vào các VLAN theo yêu cầu. VLAN có thể được cấu hình tĩnh hay động. Trong cấu hình tĩnh, người quản trị mạng phải cấu hình cho từng cổng của mỗi switch. Sau đó, gán cho nó vào một VLAN nào đó. Trong cấu hình động mỗi cổng của switch có thể tự cấu hình VLAN cho mình dựa vào địa chỉ MAC của thiết bị được kết nối vào.
## **2) Cấu hình VLAN**
- Tạo **VLAN** :
    ```
    Switch(config) # vlan [vlan-id]
    Switch(config-vlan) #
    ```
    - Dải giá trị **vlan-id** chạy từ `0` -> `4095` , trong đó :
        - `1` -> `1001` : dải **VLAN** thông thường , là dải **VLAN** thường được sử dụng
        - `1002` -> `1005` : dải **VLAN** dùng để giao tiếp với các kiểu mạng LAN khác
        - `1006` -> `4094` : dải **VLAN** mở rộng , dải này chỉ có thể sử dụng khi Switch hoạt động ở chế độ **Transparent**
        - `0` và `4095` : không sử dụng
        - Mặc định , các **VLAN** `1` , `1002` -> `1005` luôn tồn tại trên Switch , không thể xóa , sửa các **VLAN** này
- Đặt tên cho **VLAN** : 
    ```
    Switch(config-vlan) # name [vlan_name]
    ```
- Gán interface trên Switch vào **VLAN** :
    ```
    Switch(config) # interface [name]
    Switch(config-if) # switchport access vlan [vlan-id]
    ```
- Xem cấu hình **VLAN** đã thực hiện
    ```
    Switch # show vlan
    ```
## **3) Trunking**
- Trong trường hợp số lượng **VLAN** tăng lên nhiều hơn , số lượng đường nối để đấu nối giữa các VLAN cũng sẽ tăng lên tương ứng => không hợp lý .<br>=> **Giải pháp** : sử dụng chỉ 1 đường đấu nối giữa 2 Switch mà vẫn đảm bảo thông suốt giữa các LAN . Đường đấu nối này sẽ đảm bảo lưu lượng các **VLAN** đều có thể đi qua nó để đến được **VLAN** tương ứng của Switch đầu kia . Đường đấu nối vậy được gọi là đường **trunk** .

    <img src=https://i.imgur.com/ajrhYkA.png>

- Hai cổng trên 2 Switch ở đầu đường **trunk** gọi là các ***cổng*** **trunk** , ngược lại , những cổng thuộc về 1 **VLAN** nào đó , được sử dụng để kết nối đến các end-user gọi là ***cổng*** **access** .

- Mấu chốt của kỹ thuật **trunking** là đánh dấu để phân biệt giữa các **frame** của **VLAN** khác nhau khi chúng đi trên đường **trunk** . Có 2 kỹ thuật chèn thêm thông tin vào Ethernet **frame** khi nó đi vào đường **trunk** để cho biết **frame** đó thuộc **VLAN** nào , đó là **IEEE 802.1Q** ( thường được gọi tắt là **dot1q** ) và **ISL** ( **Interswitch Link** ) . **Dot1q** là chuẩn quốc tế của **IEEE** , còn **ISL** là chuẩn riêng của Cisco , chỉ chạy trên các thiết bị Cisco .
- **Kỹ thuật trunking dot1q** thực hiện chèn thêm `4 byte` thông tin trunking vào ngay sau trường **source MAC** của Ethernet **frame** khi nó đi vào trong đường **trunk** , thông tin chèn thêm vào gọi là **Dot1q Tag** . **Dot1q Tag** gồm 1 số trường như sau : 
    - **Tag Protocol Identifier ( TFI )** , dài `16 bit` : nội dung của trường này được thiết lập là `0x8100` .

    - **Class of Service ( CoS )** , dài `3 bit` : đây là `3 bit` được sử dụng cho kỹ thuật **QoS** trên Switch , việc sử dụng các bit này được tuân theo chuẩn classification ***IEEE 802.1p*** .
    - **Canonical Format Indicator ( CFI )** , dài `1 bit` : là 1 chỉ báo cho biết các địa chỉ MAC được sử dụng ở định dạng Ethernet hay TokenRing . Bit này dùng cho trường hợp mạng nội bộ có giao tiếp với loại mạng LAN TokenRing
    - **VLAN Identifier** , dài `12 bit` : trường này sẽ cho biết **frame** đang chạy trên đường **trunk** đến **VLAN** nào .
- **Kỹ thuật trunking ISL**
    - Nếu kỹ thuật trungking **dot1q** của **IEEE** chèn thêm `4 byte` thông tin trunking vào giữa Ethernet **frame** thì kỹ thuật trunking **ISL** của Cisco lại thực hiện đóng gói toàn bộ Ethernet **frame** giữa 1 header `26 byte` và 1 trường kiểm tra lỗi **CRC** dài `4 byte` .
- **Native VLAN**
    - **Frame** thuộc về **native VLAN** khi đi vào đường **trunk** sẽ không bị tag thêm thông tin trunking mà sẽ được để nguyên dạng Ethernet Frame như bình thường .

    - **Native VLAN** được sử dụng để cho phép tương thích với các thiết bị cũ không có khả năng trunking .
    - Để đảm bảo đường **trunk** hoạt động đúng đắn , 2 Switch hai đầu đường **trunk** phải cấu hình thống nhất với nhau về **native VLAN** được sử dụng trên đường **trunk** nối giữa chúng .
    - Mặc định , **native VLAN** được sử dụng là **VLAN 1** .
## **4) Cấu hình Trunking**
- Chọn kỹ thuật **trunking** : **dot1q** hay **ISL** :
    ```
    Switch(config-if) # switchport trunk encapsulation [dot1q|ISL]
    ```
- Chọn mode **trunking** :
    ```
    Switch(config-if) # switchport mode [access|trunk|dynamic|desirable|dynamic auto]
    ```

    Bảng tương tác giữa các mode :

    | | Access | Trunk | Desirable | Auto |
    |-|--------|-------|-----------|------|
    | **Access** | Access | - |  Access | Access |
    | **Trunk** | - | Trunk | Trunk | Trunk |
    | **Desirable** | Access | Trunk | Trunk | Trunk |
    | **Auto** | Access | Trunk | Trunk | Access |
    
    **Desirable** và **Auto** là các mode **trunk** tự động trên cổng . Cổng ở mode **Desirable** sẽ chủ động tương tác với đầu kia để thiết lập **trunk** , còn cổng **Auto** sẽ thụ động chờ đợi .
    => Đây là giao thức **DTP - Dynamic Trunk Protocol** .
- Tắt **DTP** trên cổng :
    ```
    Switch(config-if) # switchport mode trunk
    Switch(config-if) # switchport nonegotiate
    ```
- Giới hạn **VLAN** đi qua **trunk** : 
    ```
    Switch(config-if) # switchport trunk allowed vlan [vlan-list|all|add/except/remove vlan-list]
    ```
    - Trong đó : 
        - **vlan-list** : là 1 danh sách các **VLAN** trong danh sách được phân cách bởi dấu "`,`" hoặc dấu  "`-`"
        - **all** : tất cả các **VLAN** từ `1` đến `4094` sẽ được phép đi qua đường **trunk** .
        - **add vlan-list** : thêm 1 danh sách **VLAN** mới vào danh sách hiện có .
        - **except vlan-list** : tất cả các **VLAN** đều được phép đi qua đường **trunk** này ngoại trừ các **VLAN** nằm trong list này .
        - **remove vlan-list** : gỡ bỏ các **VLAN** trong **vlan-list** ra khỏi danh sách các **VLAN** hiện đang được đi qua đường **trunk** .
- Hiệu chỉnh **Native VLAN** ( mặc định là **VLAN 1** ) : 
    ```
    Switch(config-if) # switchport trunk native vlan [vlan-id]
    ```
- Kiểm tra kết quả cấu hình :
    ```
    Switch # show interface [name] trunk
    Switch # show interface switchport
    Switch # show trunk
    ```