# Telnet - Teletype Network
## **1) Khái niệm**
- Là 1 kỹ thuật cho phép truy nhập từ xa thiết bị để thực hiện các tác vụ cấu hình mà không cần cắm dây console trực tiếp lên thiết bị .
- Là 1 giao thức chạy trên nền `TCP` , sử dụng port `23` .
- Hoạt động theo mô hình ***Client-Server*** : trên thiết bị được Telnet phải được tích hợp dịch vụ **Telnet Server** và trên host tiến hành telnet phải tích hợp **Telnet Client** . Cisco IOS tích hợp cả 2 dịch vụ trên .
- Lưu lượng Telnet được tiếp nhận qua đường logic có tên là **VTY - Virtual Teletype** .
- Trên Router , các đường **VTY** có số hiệu từ `0` đến `4` ( 5 sessions ) .
- Trên Switch , các đường **VTY** có số hiệu từ `0` đến `15` ( 16 sessions ) .
## **2) Cấu hình Telnet**
<img src=https://i.imgur.com/9rOCdz7.png>

- Trên R1 , cấu hình cổng **VTY** cho phép Telnet :
    ```
    R1(config) # line vty 0 4
    R1(config-line) # password [password]
    R1(config-line) # login
    R1(config-line) # exit
    ```
- Truy nhập R1 từ PC, Router
    ```
    PC> telnet 192.168.1.1
    hoặc
    R2# telnet 192.168.1.1
    ```
- Hiển thị các phiên kết nối trên Telnet Server ( R1 ) :
    ```
    R1# show sessions
    ```
- Ngắt phiên kết nối của 1 Client
    ```
    R1# disconnect [VTY number]
    hoặc
    R1# show users
    => R1# clear line [VTY number]
>### ***Chú ý***
- Mặc định trên Windows không cài **Telnet Client** .
- Vào **Control Panel => Program and Features => Turn Windows Feature on or off => Chọn Telnet Client => OK** để cài đặt

    <img src=https://i.imgur.com/Ef2v3BI.png>