# Các kỹ thuật chia IP
## **1) Kỹ thuật chia subnet**
- **Nguyên lí cơ bản** : Để có thể chia nhỏ một mạng lớn thành nhiều mạng con bằng nhau , người ta mượn thêm 1 số bit bên phần `host` để làm phần mạng , các bit mượn này được gọi là các bit `subnet` . Tùy thuộc vào số bit subnet , mà có thể chia được số lượng mạng con khác nhau với các kích cỡ khác nhau .
- **VD :** *Thực hiện chia mạng `192.168.1.0 /24` thành 4 `subnet` bằng cách mượn thêm 2 bit của phần host*
```
Với các bit mượn là "00" :
        192.168.1.|00|000000 -> 192.168.1.0/26        Địa chỉ Network
        192.168.1.|00|000001 -> 192.168.1.1/26        Địa chỉ Host
        192.168.1.|00|000010 -> 192.168.1.2/26        Địa chỉ Host
        ...
        192.168.1.|00|111110 -> 192.168.1.62/26       Địa chỉ Host
        192.168.1.|00|111111 -> 192.168.1.63/26       Địa chỉ Broadcast

Với các bit mượn là "01" :
        192.168.1.|01|000000 -> 192.168.1.64/26       Địa chỉ Network
        192.168.1.|01|000001 -> 192.168.1.65/26       Địa chỉ Host
        192.168.1.|01|000010 -> 192.168.1.66/26       Địa chỉ Host
        ...
        192.168.1.|01|111110 -> 192.168.1.126/26      Địa chỉ Host
        192.168.1.|01|111111 -> 192.168.1.127/26      Địa chỉ Broadcast

Với các bit mượn là "10" :
        192.168.1.|10|000000 -> 192.168.1.128/26      Địa chỉ Network
        192.168.1.|10|000001 -> 192.168.1.129/26      Địa chỉ Host
        192.168.1.|10|000010 -> 192.168.1.130/26      Địa chỉ Host
        ...
        192.168.1.|10|111110 -> 192.168.1.190/26      Địa chỉ Host
        192.168.1.|10|111111 -> 192.168.1.191/26      Địa chỉ Broadcast

Với các bit mượn là "11" :
        192.168.1.|11|000000 -> 192.168.1.192/26      Địa chỉ Network
        192.168.1.|11|000001 -> 192.168.1.193/26      Địa chỉ Host
        192.168.1.|11|000010 -> 192.168.1.194/26      Địa chỉ Host
        ...
        192.168.1.|11|111110 -> 192.168.1.254/26      Địa chỉ Host
        192.168.1.|11|111111 -> 192.168.1.255/26      Địa chỉ Broadcast

=> Các subnet được chia ra là : 192.168.1.0/26
                                192.168.1.64/26
                                192.168.1.128/26
                                192.168.1.192/26
```
- Nhận xét
    - Với mỗi dải bit mượn , ta chia được 1 `subnet`
    - Tổng số giá trị có thể có của 1 dãy nhị phân 6 bit là 2^6 giá trị . Ta bỏ ra 2 giá trị `000000` ( *Địa chỉ Network* ) và `111111` ( *Địa chỉ Broadcast* ) thì số lượng địa chỉ dùng được cho host của 1 subnet là `2^6-2= 62` địa chỉ.
## **2) VLSM - Variable Length Subnet Mask**
- Với phương pháp chia subnet , một mạng lớn được chia thành các mạng nhỏ có kích thước bằng nhau . Trong nhiều trường hợp , việc chia như đều như vậy không đáp ứng được yêu cầu về quy hoạch IP cho sơ đồ mạng .
- Một sơ đồ **VLSM** là 1 sơ đồ tồn tại các subnet của cùng 1 mạng sử dụng các `subnet-mask` có chiều dài thay đổi , hay có thể nói , là có số `prefix-length` khác nhau . 
- **VD :** Cho dải mạng sau `10.0.0.0 /16`
    - *Yêu cầu cấp dải mạng thích hợp cho nhu cầu từng phòng ban :*
        - ***IT** : 50 hosts*
        - ***Marketing** : 100 hosts*
        - ***R&D** : 200 hosts*

    <img src=https://i.imgur.com/lPqf9zZ.png>

    ```
    Ban đầu dải mạng được cấp : 10.0.0.0/16
    Có 10.0 (16bit) là phần NetID và 0.0 (16bit) là phần HostID
    1) Chia mạng 10.0.0.0/24 cho 200 hosts
        - Có công thức : 2^n - 2 >= 200
        ( với n là số bit phần HostID , 200 là số lượng IP yêu cầu )
        <=> n=8 vì 2^8=254 > 200 host yêu cầu
        Vậy HostID = 8
        Mà HostID + NetID = 32bit => NetID = 24bit ( /24 )
        - Dải sau khi chia cho 200 host sẽ có 24bit NetID và 8bit HostID vậy ở đây NetID sẽ mượn 8bit từ HostID ban đầu để làm NetID
        - Dải ban đầu là : 10.00000000|00000000.00000000
        - Dải sau khi chia cho 200 hosts : 10.00000000.00000000|00000000
        - 8bit của HostID sẽ chạy từ 00000001 đến 11111110 (254 host)
        Vậy dải 10.0.0.0/24 sẽ có các host chạy từ 10.0.0.1 -> 10.0.0.254 chứa được 200 host của phòng R&D
        - Dải tiếp nối của dải mạng này là 10.0.00000001.00000000/24 <=> 10.0.1.0/24
    2) Chia mạng 10.0.1.0/24 cho 100 host
        - Tương tự có 2^n - 2 >= 100 <=> n=7 vì 2^7=128 > 100 host yêu cầu => HostID = 7
                                                                        => NetID = 25 ( /25 )
        - Dải ban đầu là 10.0.00000001|00000000
        - Dải sau khi chia cho 100 hosts : 10.0.00000001.0|0000000
        - 7 bit của HostID sẽ chạy từ 0000001 đến 1111110 (126 host)
        Vậy dải 10.0.1.0/25 sẽ có các host chạy từ 10.0.1.1 -> 10.0.1.126 chứa được 100 host của phòng Marketing
        - Dải tiếp nối của dải mạng này là 10.0.1.1|0000000/25 <=> 10.0.1.128/25
    3) Chia mạng 10.0.1.128/25 cho 50 host
        - Tương tự có 2^n - 2 >= 50 <=> n=6 vì 2^6=64 > 50 host yêu cầu => HostID = 6
                                                                        => NetID = 26 ( /26 )
        - Dải ban đầu là 10.0.00000001.1|0000000
        - Dải sau khi chia cho 50 hosts : 10.0.00000001.10|000000
        - 6 bit của HostID sẽ chạy từ 000001 đến 111110 (62 host)
        Vậy dải 10.0.1.128/26 sẽ có các host chạy từ 10.0.1.129 -> 10.0.1.190 chứa được 50 host của phòng IT

    => Kết quả :     + IT : 10.0.1.128/26
                    + Marketing : 10.0.1.0/25
                    + R&D : 10.0.0.0/24
    ```
- Có thể dùng công cụ `vlsm-calc` trên Internet để chia **VLSM**
    - Truy cập http://www.vlsm-calc.net/
    - Nhập thông tin mạng cần chia :

        <img src=https://i.imgur.com/8ninR4L.png>
    
    - Kết quả cho thấy tương tự việc chia bằng tay :

        <img src=https://i.imgur.com/ffjFp79.png>

## **3) CIDR - Classless Inter-Domain Routing**
- Là kỹ thuật tóm tắt địa chỉ ( **summary**)
- Nếu kỹ thuật chia subnet thực hiện chia 1 mạng lớn thành nhiều mạng nhỏ ( `subnet` ) thì kỹ thuật **CIDR** - **summary** lại thực hiện gộp nhiều mạng nhỏ thành 1 mạng lớn . 
- Nguyên lý : Để summary nhiều network , tiến hành quan sát các `octet` của các địa chỉ này từ trái sang phải và xét `octet` khác nhau đầu tiên . Thực hiện phân tích nhị phân các `octet` khác nhau đầu tiên này và tiếp tục chọn ra các bit nhị phân giống nhau trong các `octet` . Phần network của địa chỉ **summary** sẽ được tạo thành từ các `octet` giống nhau trước đó và các bit nhị phân giống nhau của `octet` vừa xét .
- **VD :** *Hãy tóm tắt ( **summary** ) 4 địa chỉ mạng sau đây thành 1 địa chỉ mạng duy nhất*
    - `192.168.0.0/24`
    - `192.168.1.0/24`
    - `192.168.2.0/24`
    - `192.168.3.0/24`
    ```
    - Có thể thấy rằng 4 địa chỉ trên giống nhau các octet thứ 1 và thứ 2, khác nhau ở octet thứ 3 . Thực hiện phân tích nhị phân octet thứ 3 : 
                192.168.|000000|00.0
                192.168.|000000|01.0
                192.168.|000000|10.0
                192.168.|000000|11.0
    - Ở octet thứ 3, các địa chỉ này còn giống nhau thêm được 6 bit nữa. 
    Vậy địa chỉ network mà bao trùm 4 địa chỉ network trên sẽ có phần network bao gồm octet thứ 1, octet thứ 2 và thêm 6 bit giống nhau kia nữa. 
    Cho các bit còn lại làm phần host và clear chúng về 0, ta sẽ có được địa chỉ summary cần tìm là 192.168.0.0/22
    ```
