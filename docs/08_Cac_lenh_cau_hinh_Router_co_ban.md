# Các lệnh cấu hình Router cơ bản
## **1) Các thành phần của Router**
<img src=https://i.imgur.com/aMXrzZk.jpg>

- **CPU** : bộ xử lý trung tâm.
- **ROM** : chứa chương trình kiểm tra khởi động ( POST ) , Bootstrap ( giống BIOS của máy tính ) và mini-IOS ( recovery password , upgrade IOS ) . Nhiệm vụ chính của **ROM** là kiểm tra phần cứng khi khởi động , sau đó chép HĐH Cisco IOS từ **FLASH** vào **RAM** . Nội dung trong bộ nhớ **ROM** là không thể xóa được .
- **RAM / DRAM** : lưu trữ routing table , ARP cache , fast-switching cache , packet buffering ( shared RAM ) và packet hold queues . Đa số HĐH Cisco IOS chạy trên **RAM** . **RAM** còn lưu trữ cấu hình đang chạy của Router ( running-config ) . Nội dung lưu trên RAM bị mất khi tắt nguồn hoặc restart Router .
- **FLASH** : lưu trữ toàn bộ HĐH Cisco IOS ( giống ổ cứng ) 
.
- **NVRAM** - Non-volatile RAM : lưu trữ file cấu hình backup / startup của Router ( startup-config ) ; nội dung của **NVRAM** vẫn được giữ sau khi tắt nguồn hoặc bật / tắt Router .
- **Interface** : còn gọi là cổng , được kết nối trên board mạch chủ hoặc trên interface module riêng biệt , qua đó những packet đi vào và đi ra Router . Cổng ` Console` sử dụng cáp `Rollover` , dùng để cấu hình trực tiếp Router . Cổng `AUX` giống với cổng `Console` nhưng sử dụng kết nối Dial-up tới modem , hỗ trợ việc cấu hình từ xa . Còn lại là các cổng mạng thông thường : `Gigabit Ethernet` , `Fast Ethernet` , `Serial`,...
## **2) Các chế độ cấu hình Router Cisco**
- Có 3 chế độ cấu hình cơ bản 
    - **> User Mode** : cho phép các câu lệnh hiển thị thông tin 1 cách hạn chế, các lệnh kết nối ( ping , traceroute , telnet , ssh ,... )
    - **# Priviledged Mode** : cho phép toàn bộ câu lệnh hiển thị , một số cấu hình cơ bản ( clock , copy , erase ,... )
    - **Global Configuration Mode ( config )#** :cho phép toàn bộ các câu lệnh.
### **2.1) Priviledged Mode**
- Để vào **Priviledged Mode**
    ```
    Router > enable
    Router #
    ```
- Hiển thị thông tin Router : 
    ```
    Router # show flash
    ```
- Hiển thị phiên bản Router :
    ```
    Router # show version
    ```
- Hiển thị thông tin các cổng kết nối có trên Router :
    ```
    Router # show ip interface brief
    ```
- Kiểm tra kết nối từ Router đến IP 10.10.10.10
    ```
    Router # ping 10.10.10.10
    ```
- Kiểm tra tuyến đường từ Router đến IP 10.10.10.10
    ```
    Router # traceroute 10.10.10.10
    ```
- Hiển thị các câu lệnh đang áp dụng trên Router
    ```
    Router # show running-config
    ```
- Hiển thị cấu hình đã lưu ở NVRAM - cấu hình khởi động
    ```
    Router # show startup-config
    ```
- Xóa cấu hình khởi động của Router
    ```
    Router # erase startup-config
    ```
- Khởi động lại Router
    ```
    Router # reload
    ```
- Sử dụng lệnh `exit` hoặc `disable` để đi từ các mode bên trong ra bên ngoài . Hoặc có thể dùng lệnh `end` hoặc `Ctrl` + `Z` để đi từ sub-mode ra ngoài **Priviledged Mode**.
### **2.2) Global Configuration Mode**
- Để vào `(config)#` : chế độ cấu hình chi tiết cho Router
    ```
    Router # configure terminal
    Router(config) #
    ```
- Đặt hostname cho Router
    ```
    Router(config)# hostname R1
    R1(config)#
    ```
- Cấu hình chống trôi dòng lệnh
    ```
    Router(config) # line console 0
    Router(config-line) # logging synchronous
    ```
    => Tắt chức năng chống trôi dòng lệnh
    ```
    Router(config-line) # no logging synchronous
    ```
- Đặt password enable cho Router

    - Password dạng ***ClearText***
    ```
    Router(config) # enable password [password]
    ```
    - Password dạng ***MD5***
    ```
    Router(config) # enable secret [password]
    ```
- Mã hóa toàn bộ mật khẩu trên Router
    ```
    Router(config) # service password-encryption
    ```
    => Tất cả password đã được mã hóa . Mật khẩu `enable secret` vẫn ở dạng cũ ; mức độ mã hóa được hiển thị bằng chỉ số đứng trước mật khẩu .
    - **7** : mật khẩu được mã hóa theo thuật toán 2 chiều **MD7** ( có thể giải mã được )
    - **5** : mật khẩu được mã hóa theo thuật toán 1 chiều **MD5**
    - **0** ( hoặc không có giá trị ) : mật khẩu không được mã hóa.
- Đặt password cho cổng `Console` : 
    ```
    R1(config) # line console 0
    R1(config-line) # password [password]
    R1(config-line) # login
    R1(config-line) # exit
- Đặt password cho cổng `Auxiliary`
    ```
    R1(config) # line aux 0
    R1(config-line) # password [password]
    R1(config-line) # login
    R1(config-line) # exit
    ```
- Đặt password cho cổng `Telnet`
    ```
    R1(config) # line vty 0 4
    R1(config-line) # password [password]
    R1(config-line) # login
    R1(config-line) # exit
    ```
- Tạo **Login Banner** / **Motd Banner**
    ```
    Router(config) # banner motd "This is banner motd"
    Router(config) # banner login "This is banner login"
    ```
- Cấu hình IP cổng interface : 
    ```
    Router(config) # interface [name]
    Router(config-if) # description [describe]
    Router(config-if) # ip address [IP] [subnet-mask]
    Router(config-if) # no shutdown
    Router(config-if) # exit
    ```
-  Cấu hình Timezone : 
    ```
    Router(config) # clock timezone UTC 7
    ```
    => Show clock
    ```
    Router # show clock
    ```
- Lưu file cấu hình đang chạy vào file khởi động : 
    ```
    Router # copy running-config startup-config
    ```
    hoặc
    ```
    Router # write
    ```
- Cấu hình thời gian timeout cho cổng `Console` : 
    ```
    Router(config) # line console 0
    Router(config-line) # exec-timeout 0 0    ( minute second )
    ```
- Hiển thị thông tin phần cứng của interface : 
    ```
    Router # show controllers serial 0/0/0
    ```
- Hiển thị thông tin các user đang kết nối trực tiếp vào thiết bị ( qua telnet , ssh): 
    ```
    Router # show users
    ```
- Hiển thị lịch sử các câu lệnh đã thực thi trên Router :
    ```
    Router # history
    ```

