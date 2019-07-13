# DHCP - Dynamic Host Configuration Protocol
## **1) Khái niệm**
- Dịch vụ **DHCP** cung cấp địa chỉ IP tự động cho các thiết bị trong mạng . Đây là 1 dịch vụ được sử dụng phổ biến đơn giản hóa trong việc cấu hình , quản lý địa chỉ mạng .
- **DHCP** hoạt động theo dạng ***Client - Server*** . Máy chủ đóng vai trò DHCP Server được cấu hình các tham số để cấp phát IP động . Các tham số này gồm : ***tên*** , ***địa chỉ mạng*** , ***dãy địa chỉ cấp phát*** , ***default-gateway*** , ***DNS Server*** , ***thời gian người dùng được sử dụng IP ( lease-time )*** .
## **2) Nguyên tắc hoạt động**
- **B1** : **DHCP Client** gửi gói tin **DHCP DISCOVER** dạng ***Broadcast*** đến **DHCP Server**
- **B2** : **DHCP Server** gửi lại gói tin **DHCP OFFER** dạng ***Unicast*** cho **DHCP Client**
- **B3** : **DHCP Client** gửi gói **DHCP REQUEST** dạng ***Broadcast*** cho **DHCP Server**
- **B4** : **DHCP Server** gửi gói **DHCP ACK** dạng ***Unicast*** cho **DHCP Client**

    <img src=https://i.imgur.com/j92zuND.png>

## **3) Các gói tin phụ của DHCP**
- Gói tin **DHCP NAK** : Nếu 1 địa chỉ IP đã hết hạn hoặc đã được cấp phát cho 1 Client khác , DHCP Server sẽ tiến hành gửi gói tin này cho Client . Như vậy , nếu Client muốn sử dụng lại địa chỉ IP thì phải bắt đầu tiến trình thuê lại địa chỉ IP.
- Gói tin **DHCP DECLINE** : Nếu DHCP Client nhận được bản tin trả về không đủ thông tin hoặc hết hạn , nó sẽ gửi gói **DHCP DECLINE** đến các DHCP SERVER để yêu cầu thiết lập lại tiến trình thuê IP .
- Gói tin **DHCP RELEASE** : Client gửi bản tin này đến Server để ngừng thuê IP . Khi nhận được bản tin này , Server sẽ thu hồi lại IP đã cấp cho Client
## **4) Cấu hình Router thành DHCP Server**
<img src=https://i.imgur.com/Nqz1EOU.png>

- Khởi tạo dịch vụ **DHCP** :
    ```
    R1(config) # service dhcp
    ```
- Xác định dải địa chỉ mà IP sẽ cấp phát cho Client :
    ```
    R1(config) # ip dhcp pool [name]
    R1(config-dhcp) # network [network] [subnet-mask]
    ```
- Xác định **Default-Gateway** của dải mạng :
    ```
    R1(config-dhcp) # default-router 192.168.1.1
    ```
- Xác định **DNS Server** :
    ```
    R1(config-dhcp) # dns-server 8.8.8.8
    ```
- Loại trừ dãy IP không cấp phát :
    ```
    R1(config) # ip dhcp excluded-address [start IP] [end IP]
    ```
- Kiểm tra IP đã cấp phát từ R1 :
    ```
    R1 # show ip dhcp binding
    ```
- Kiểm tra trạng thái các gói tin **DHCP** đến và đi trên Server :
    ```
    R1 # show ip dhcp server statistics
    ```
- Trên PC , mở **CMD ( Command Promt )** thực hiện nhận IP từ DHCP Server :
    ```
    > ipconfig /release            # trả lại IP đã thuê
    > ipconfig /renew              # nhận về IP mới
    ```
## **5) Cấu hình Router thành DHCP Client**
<img src=https://i.imgur.com/SMgXB0q.png>

- R1 đã cấu hình **DHCP Server** có với dải `192.168.12.0/24` có Gateway là `192.168.12.1` 

- Trên cổng `f0/0` của R2 :
    ```
    R2(config) # interface f0/0
    R2(config-if) # ip address dhcp
    R2(config-if) # no shutdown
    R2(config-if) # exit
    ```