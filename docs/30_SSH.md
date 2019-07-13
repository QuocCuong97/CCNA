# SSH - Secure Socket Shell
## **1) Khái niệm**
- Là giải pháp truy nhập từ xa có tính bảo mật cao thường được dùng để thay thế Telnet vì trong Telnet , dữ liệu được trao đổi giữa **Client** và **Server** được để ở dạng ***ClearText*** , dẫn đến có thể bị xem trộm .
- Là 1 ứng dụng chạy nền `TCP` , sử dụng port `22` .
- **SSH** có 2 version `1` và `2` không tương thích với nhau . Ver `2` được khuyến nghị sử dụng vì an toàn hơn ver `1` .
## **2) Cấu hình SSH trên Router**
<img src=https://i.imgur.com/d8IoWpq.png>

- Cấu hình 1 domain-name :
    ```
    R1(config) # ip domain-name [name]        ( VD : cisco.com )
    ```
- Cấu hình tài khoản truy nhập bằng **SSH** :
    ```
    R1(config) # username [username] password [password]
    ```
- Cấu hình Router phát sinh key cho việc mã hóa dữ liệu :
    ```
    R1(config) # crypto key generate rsa
    ```
- Thiết lập **VTY** hỗ trợ truy nhập bằng **SSH** :
    ```
    R1(config) # line vty 0 4
    R1(config-line) # transport input ssh
    R1(config-line) # login local
    R1(config-line) # exit
    ```
- Kết nối từ Router khác vào Router đã cấu hình **SSH** :
    ```
    R# ssh -l [username] [Domain/IP_SSH_Server]
    => Nhập password ___
    ```
- Kết nối bằng Client Windows : sử dụng **PuTTy**
    - Nhập IP hoặc hostname của **SSH Server** ( 192.168.1.1 )
    - Chọn giao thức kết nối ( `SSH` ) và port ( `22` )
    - Nhấn `Open` để mở phiên kết nối

        <img src=https://i.imgur.com/PQaLJRe.png>
