# CDP - Cisco Discovery Protocol
- Là 1 giao thức đặc biệt của Cisco cho phép thu thập thông tin về thiết bị láng giềng kết nối trực tiếp với mình .
- Khi 1 thiết bị Cisco được bật lên , nó sẽ gửi ra tất cả các cổng mạng của nó thông điệp **CDP** mang các thông tin về bản thân để các thiết bị láng giềng có thể nắm được thông tin về nó .
- Được gửi theo chu kỳ mặc định `60s/lần` .
- Chỉ chạy được khi các cổng ở trạng thái ***up/up*** .
- **CDP** cho biết những thông tin sau về thiết bị láng giềng :
    - **Device ID :** hostname của thiết bị láng giềng
    - **Local Interface :** cổng được dùng để kết nối tới láng giềng .
    - **Outgoing Port :** láng giềng dùng cổng nào để kết nối tới mình .
    - **Capability :** láng giềng có khả năng gì ( Router , Switch ,...)
    - **Platform :** chủng loại của thiết bị láng giềng .
    - **Địa chỉ IP** của láng giềng .
    - **IOS :** hệ điều hành của láng giềng .
## **Các thao tác với CDP**
- Xem thông tin láng giềng :
    ```
    Router# show cdp neighbor
    ```
    <img src=https://i.imgur.com/D6hq2XG.png>
- Xem thông tin chi tiết về láng giềng :
    ```
    Router# show cdp neighbor detail
    Router# show cdp entry *
    ```
    <img src=https://i.imgur.com/83w5eYz.png>
    
- Bật / Tắt **CDP** trên các cổng :
    ```
    Router(config) # [no] cdp run
    ```
- Bật / Tắt **CDP** trên 1 cổng nào đó của thiết bị :
    ```
    Router(config-if) # [no] cdp enable
    ```