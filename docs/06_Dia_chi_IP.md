# Địa chỉ IP
## **1) Khái niệm IP - Internet Protocol**
- Trong một hệ thống mạng, các máy tính hoặc các thiết bị điện tử liên lạc với nhau thông qua IP.

- Địa chỉ IP có 2 version : **IPv4** ( 32bit ) và **IPv6** ( 128 bit ).
## **2) Hình thức IPv4 và Subnetmask**
- Địa chỉ **IPv4** là 1 dải nhị phân dài 32bit và chia thành 4 bộ 8bit gọi là các `Octet`, gồm phần `net-id` dùng để xác định mạng mà thiết bị kết nối và phần `host-id` để xác định thiết bị của mạng đó.
- Cấu trúc  :    ` Số . Số . Số . Số`   ( 0 <= Số <= 255 )
- Mỗi 1 ` Số ` gọi là 1 `Octet` ( 1 `octet` = 8 bit ).
- Mỗi địa chỉ IP luôn đi kèm 1 `subnet-mask`, để xác định phần `net-id` của địa chỉ đó. `Subnet-mask` cũng là 1 dải nhị phân dài 32bit và chia ra 4 bộ 8bit như địa chỉ IP.
- `Subnet-mask` bao gồm phần các bit `1` và phần còn lại là các bit `0`, `subnet-mask` có bao nhiêu bit `1` thì địa chỉ IP tương ứng có bấy nhiêu phần `net-id`.

    **VD:** Có thể viết như sau : ```192.168.1.3 - 255.255.255.0```
                hoặc sử dụng ***prefix-length*** : ```192.168.1.3 / 24```
- Bảng `Subnet-mask`

    <img src=https://i.imgur.com/CqZ9MXz.png>
    
## **3) Các lớp địa chỉ IP**
### **3.1) Lớp A**

<img src=https://i.imgur.com/vPKEvtx.png>

- Địa chỉ lớp A sử dụng 1 `octet` làm `net-id`, 3 octet sau làm `host-id`.
- Bit đầu của 1 địa chỉ lớp A luôn được giữ là `0`.
- Các địa chỉ mạng lớp A gồm `1.0.0.0` ->  `126.0.0.0`.
- Mạng `127.0.0.1` được sử dụng làm `IP Loopback`.
- Phần `host` có 24bit =>  Mỗi mạng lớp A có ( 2^24 - 2 ) = 16,777,214 hosts.
### **3.2) Lớp B**

<img src=https://i.imgur.com/gee8EMQ.png>

- Địa chỉ lớp B sử dụng 2 `octet` đầu làm  `net-id` , 2 `octet` sau làm `host-id`.
- 2 bit đầu của địa chỉ lớp B luôn được giữ là `10`.
- Các địa chỉ mạng lớp B bao gồm `128.0.0.0` -> `191.255.0.0`.
- Có tất cả 2^14 mạng trong lớp B.
- Phần `host` dài 16bit => Mỗi mạng lớp B có ( 2^16 - 2 ) = 65534 hosts.
### **3.3) Lớp C**

<img src=https://i.imgur.com/2BK8JVu.png>

- Địa chỉ lớp C sử dụng 3 `octet` đầu làm `net-id`, `octet` cuối là `host-id`.
- 3 bit đầu của 1 _địa chỉ lớp C_ luôn được giữ là `110`.
- Các địa chỉ mạng lớp C gồm `192.0.0.0` -> `223.255.255.0`.
- Có tất cả 2^21 mạng trong lớp C.
- Phần `host` dài 8 bit do đó 1 mạng lớp C có ( 2^8 -2 ) = 254 hosts.
### **3.4) Lớp D**

<img src=https://i.imgur.com/0i3LisH.png>

- Gồm các địa chỉ thuộc dải `224.0.0.0` -> `239.255.255.255`.
- Được sử dụng làm địa chỉ **Multicast**.
- **VD:** 
    -  `224.0.0.5` và `224.0.0.6` được dùng cho **OSPF**.
    - ` 224.0.0.9` được dùng cho **RIPv2**.
    - `224.0.0.10` được dùng cho **EIGRP**.
### **3.5) Lớp E**

<img src=https://i.imgur.com/JI3duN5.png>

- Từ `240.0.0.0` trở đi.
- Được sử dụng cho mục đích dự phòng và nghiên cứu.
        
## ***Lưu ý :***
- Các lớp địa chỉ IP có thể sử dụng để đựt cho các host là thuộc các lớp A, B, C.
- Để thuận tiện cho việc nhận diện địa chỉ IP thuộc lớp nào, có thể quan sát `octet` đầu của địa chỉ, nếu `octet` này nằm trong khoảng : 
    - `1` -> `126` : địa chỉ lớp A

    - `128` -> `191` : địa chỉ lớp B
    - `192` -> `223` : địa chỉ lớp C
    - `224` -> `239` : địa chỉ lớp D
    - `240` -> `255` : địa chỉ lớp E
- Các địa chỉ địa chỉ IP đặc biệt
    - `127.0.0.0` -> `127.0.0.8` : địa chỉ Loopback
    - `169.254.0.0` -> `169.254.16` : địa chỉ link local
## **4) Phương thức gửi gói tin**
- Có 3 phương thức : 
    - **Unicast** : 1PC gửi cho 1PC
    - **Multicast** : 1PC gửi cho nhiều PC
    - **Broadcast** : 1PC gửi cho mọi PC trong cùng dải mạng
        - Local Broadcast : `255.255.255.255`
        - Direct Broadcast : `192.168.1.255`

<img src=https://i.imgur.com/hMeh8iy.png>

## **5) Phân loại IP - Private IP và Public IP**
- **Private IP** : chỉ được sử dụng trong mạng LAN, không được định tuyến trong môi trường Internet, có thể được sử dụng lặp lại trong các mạng LAN khác nhau.
    - Dải **IP Private** ( quy định trong ***RFC 1918*** )
        - Lớp A : `10.x.x.x`
        - Lớp B : `172.16.x.x` -> `172.31.x.x`
        - Lớp C : `192.168.x.x`
- **Public IP** : là địa chỉ IP sử dụng cho các gói tin đi trên môi trường Internet , được định tuyến trên môi trường Internet . Địa chỉ Public phải là duy nhất cho mỗi host tham gia vào Internet.
- Kỹ thuật **NAT** được sử dụng để chuyển đổi giữa địa chỉ **IP Private** và **IP Public**.
- Ý nghĩa của **IP Private** : sử dụng để bảo tồn IP Public.
## **6) Địa chỉ MAC - Media Access Control**
- Thường được gọi là địa chỉ phần cứng ( ***hardware address*** ) hay địa chỉ vật lý ( ***physical address*** ) hay thông dụng nhất là **MAC**. 
- Địa chỉ MAC là loại địa chỉ duy nhất không trùng lặp với địa chỉ của bất cứ thiết bị nào trên thế giới.
- Gồm 48 bit nhị phân ( 6 bytes ) và thường được hiển thị dưới dạng hexa.
    - **OUI - Organizationally Unique Identifier** : gồm 3 byte đầu , được sử dụng để định danh nhà cung cấp thiết bị (***vendor***).
    - **NICS - Network Interface Controller Specific** : do nhà sản xuất gán cho thiết bị của mình.

        <img src=https://i.imgur.com/nSwRyYu.jpg>


