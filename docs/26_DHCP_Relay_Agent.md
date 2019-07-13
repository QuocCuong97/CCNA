# DHCP Relay Agent
## **1) Định nghĩa**
- Là 1 tính năng cấu hình cho các Router trung gian để tiếp nhận các bản tin yêu cầu cấp phát IP của Client và chuyển các IP này đến DHCP Server .
## **2) Nguyên tắc hoạt động**
- **B1 :** **Client** sẽ gửi bản tin ***Broadcast*** và gói tin **DHCP DISCOVER** trong nội bộ mạng
- **B2 :** **DHCP Relay Agent** sẽ nhận gói tin và chuyển đến **DHCP Server** bằng bản tin ***Unicast***
- **B3 :** **DHCP Server** dùng bản tin ***Unicast*** gửi về **DHCP Relay Agent** 1 gói tin **DHCP OFFER** đến các **Client**
- **B4 :** **DHCP Relay Agent** sẻ ***Broadcast*** gói tin **DHCP OFFER** đến các **Client**
- **B5 :** Sau khi nhận được gói tin **DHCP OFFER** , **Client** gửi ***Broadcast*** tiếp gói **DHCP REQUEST** cho **Relay Agent**
- **B6 :** **DHCP Relay Agent** nhận gói tin **DHCP REQUEST** đó và chuyển đến **DHCP Server** cũng bằng bản tin ***Unicast***
- **B7 :** **DHCP Server** dùng tín hiệu ***Unicast*** gửi trả **DHCP Relay Agent** 1 gói tin **DHCP ACK**
- **B8 :** **DHCP Relay Agent** sẻ ***Broadcast*** gói tin **DHCP ACK** đến **Client** để **Client** nhận được địa chỉ IP

    <img src=https://i.imgur.com/AHP0fEf.png>
## **2) Cấu hình Relay Agent**
<img src=https://i.imgur.com/GCqpQFZ.png>

- R1 đã cấu hình **DHCP Server** có với dải `10.10.10.0/24` có Gateway là `10.10.10.1`

- Trên cổng `f0/1` của R2 ( **Relay Agent** ):
    ```
    R2(config) # interface f0/1
    R2(config-if) # ip helper-address 10.10.10.1
    ```