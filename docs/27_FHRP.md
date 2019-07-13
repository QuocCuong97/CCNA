# FHRP - First Hop Redundancy Protocol
- Khi sử dụng giao thức , các **Router Gateway** sẽ cùng nhau tạo ra 1 **Router ảo** làm nhiệm vụ ***default-gateway*** cho các end user . Các end-user thay vì phải cấu hình địa chỉ ***default-gateway*** là địa chỉ của 1 trong các Router thật thì sẽ cấu hình ***default-gateway*** là địa chỉ của Router ảo vừa nêu .
- Khi end-user muốn gửi dữ liệu ra mạng bên ngoài , nó thực hiện đóng gói chuyển dữ liệu này về gateway ảo **FHRP** . Trong 2 Router tham gia **FHRP** , sẽ chỉ có 1 Router thực sự chuyển dữ liệu ra bên ngoài , Router còn lại sẽ đóng vai trò dự phòng và chỉ chuyển dữ liệu phục vụ end-user khi Router chính gặp sự cố .
- Các giao thức **FHRP** được sử dụng phổ biến ngày nay bao gồm 3 giao thức **HSRP** , **VRRP** , **GLBP** .
## **HSRP - Hot Standby Router Protocol**

<img src=https://i.imgur.com/IkDYYfB.png>

- Đây là giao thức **FHRP** của Cisco , chỉ chạy trên các thiết bị Cisco .
- Với **HSRP** , Router chính đảm nhiệm nhiệm vụ chuyển dữ liệu đi ra khỏi mạng LAN cho các end-user được gọi là **Active Router** , Router dự phòng là **Standby Router** , nếu có các Router còn lại sẽ ở trạng thái ***Listen*** dự phòng cho **Active** và **Standby** .
- Tiêu chí bầu chọn **Active Router** là giá trị `priority` được thiết lập trên cổng Ethernet đấu nối xuống LAN của các Router tham gia group : Router nào có `priority` cao nhất sẽ là **Active** , cao nhì sẽ là **Standby** , còn lại ở trạng thái ***Listen*** .
- Nếu `priority` bằng nhau , địa chỉ IP sẽ được sử dụng để bầu chọn .
- Mặc định , việc bầu chọn của **HSRP** là ***non-preempt*** , tuy nhiên , có thể thay đổi được tính chất này . Cơ chế ***non-preempt*** là cơ chế mà khi **Active / Standby** đã được bầu chọn , các Router mới vào nhóm sẽ không được chiếm quyền dù có `priority` cao hơn .
### **Cấu hình HSRP**
- Cấu hình group và IP cho **Router ảo** :
    ```
    R(config-if) # standby [group-id] ip [ip ảo]
    ```
    - Trong đó :
        - **group-id** : là số hiệu của nhóm HSRP mà 2 Router tham gia ( `0` <= group-id <= `255` )
        - **IP ảo** : là địa chỉ IP gán cho Router ảo . Với **HSRP** , địa chỉ này không được trùng IP nào trong LAN .
- Cấu hình giá trị **priority** để chọn Router đóng vai trò **Active** :
    ```
    R(config-if) # standby [group-id] priority [priority]
    ```
    - Trong đó : `0` <= **priority** <=255 , mặc định là `100`
- Tắt tính năng ***non-preempt*** :
    ```
    R(config-if) # standby [group-id] preempt
    ```
- Kiểm tra cấu hình **HSRP** :
    ```
    R# show standby brief
    ```
## **VRRP - Virtual Router Redundancy Protocol**

<img src=https://i.imgur.com/O5kmiDC.png>

- Hoàn toàn tương tự như **HSRP** .
- Là giao thức chuẩn quốc tế , có thể chạy trên nhiều sản phẩm của nhiều hãng khác nhau .
- **Active Router** trong **HSRP** sẽ là **Master Router** trong **VRRP** . Các Router còn lại sẽ đóng vai trò ***backup*** .
- Hoạt động bầu chọn tương tự **HSRP** .
- Mặc định , có tính chất ***preempt*** .
- Với **VRRP** , địa chỉ IP của Router ảo có thể trùng với IP thật của 1 trong các Router tham gia nhóm . Khi đó , Router có địa chỉ trùng với **Router ảo** sẽ đảm nhận vai trò **Master** .
- Địa chỉ MAC của Router ảo **VRRP** có dạng `0000.5e00.01XX` , trong đó `XX` là số hiệu của **group-id** **VRRP** ( số `hexa` ) .
### **Cấu hình VRRP**

- Tương tự như **HSRP** , chỉ thay `standby` bằng `vrrp` .
## **GLBP - Gateway Load Balancing Protocol**

<img src=https://i.imgur.com/4RAOoIz.png>

- Là giao thức dự phòng gateway của các thiết bị Cisco : chỉ chạy trên các thiết bị Cisco .
- **GLBP** không chỉ thực hiện dự phòng mà còn cho phép các Router cân bằng tải lưu lượng từ LAN đi ra ngoài .
- **GLBP** sử dụng 4 địa chỉ Virtual MAC cho mỗi gateway ảo được tạo ra .
- Mỗi Router tham gia nhóm **GLBP** sẽ nắm giữ 1 MAC ảo này và thực hiện chuyển dữ liệu gửi đến MAC ấy ra ngoài .