# Inter-VLAN Routing
## 1) Định tuyến VLAN sử dụng Router
- Nếu sử dụng Router thì giải pháp định tuyến này gọi là ***Router on a Stick*** .
- **VD :** 

    <img src=https://i.imgur.com/V0BzgZA.png>

    - Cấu hình để cổng `f0/1` của Switch là cổng **Trunk** :
        ```
        Switch(config) # interface f0/1
        Switch(config-if) # switchport trunk encapsulation dot1q
        Switch(config-if) # switchport mode trunk
        ```
    - Cấu hình chia `sub-interface` trên Router :
        ```
        Router(config) # interface f0/0
        Router(config-if) # no shutdown
        Router(config-if) # exit

        Router(config-if) # interface f0/0.10
        Router(config-if) # encapsulation dot1q 10
        Router(config-if) # ip address 192.168.10.1 255.255.255.0
        Router(config-if) # exit

        Router(config-if) # interface f0/0.20
        Router(config-if) # encapsulation dot1q 20
        Router(config-if) # ip address 192.168.20.1 255.255.255.0
        Router(config-if) # exit

        Router(config-if) # interface f0/0.30
        Router(config-if) # encapsulation dot1q 30
        Router(config-if) # ip address 192.168.30.1 255.255.255.0
        Router(config-if) # exit
        ```
    => Mỗi `Sub-interface` sẽ được đặt 1 địa chỉ là **gateway** cho các host nằm trong **VLAN** tương ứng mà nó đấu nối vào . Các host thuộc **VLAN** tương ứng sẽ trỏ `default-gateway` đến địa chỉ này để đi được đến các host thuộc VLAN khác thông qua Router .
## **2) Định tuyến VLAN với Switch Layer 3**
- Các Switch Layer 3 ,  bên cạnh khả năng chuyển mạch các Frame Ethernet và cấu hình các công nghệ Layer 2 như **VLAN** , **trunking** , **STP** , còn có khả năng chạy định tuyến và chuyển mạch Layer 3 dựa vào địa chỉ IP của các gói tin .
- Định tuyến trên Switch Layer 3 thông qua các **SVI - Switched Virtual Interface** , có tên gọi khác là **interface VLAN** .
- **VD :**

    <img src=https://i.imgur.com/64NbvXz.png>

    - Gateway cho các PC thuộc **VLAN** :
        - Interface VLAN10 : `192.168.10.1/24`
        - Interface VLAN20 : `192.168.20.1/24`
        - Interface VLAN30 : `192.168.30.1/24`
    - Tạo và gắn port cho các **VLAN** :
        ```
        Switch(config) # vlan 10,20,30

        Switch(config) # interface range f0/1-3
        Switch(config-if-range) # switchport access vlan 10

        Switch(config) # interface range f0/4-6
        Switch(config-if-range) # switchport access vlan 20

        Switch(config) # interface range f0/7-9
        Switch(config-if-range) # switchport access vlan 30
        ```
    - Bật tính năng định tuyến trên Switch Layer 3 :
        ```
        Switch(config) # ip routing
        ```
    - Cấu hình các **SVI** :
        ```
        Switch(config) # interface VLAN10
        Switch(config-if) # ip address 192.168.10.1 255.255.255.0
        Switch(config-if) # no shut

        Switch(config) # interface VLAN20
        Switch(config-if) # ip address 192.168.20.1 255.255.255.0
        Switch(config-if) # no shut

        Switch(config) # interface VLAN30
        Switch(config-if) # ip address 192.168.30.1 255.255.255.0
        Switch(config-if) # no shut
        ```



