# ACL - Access Control List
## **1) Công dụng**
- **Access Control List ( ACL )** , thường được gọi tắt là **access-list** , là 1 danh sách điều khiển truy nhập .
- **Access-list** thường được sử dụng cho 2 mục đích :
    - Lọc lưu lượng
    - Phân loại dữ liệu

    <img src=https://i.imgur.com/uTMbP06.png>

## **2) Các loại Access-list**
- **Standard ACL** : 
    - Chỉ đề cập ddeens`source IP` của gói tin , không đề cập đến các thông tin khác .
    - Dải số từ :
        - `1` -> `99`
        - `1300` -> `1999`
    - Chặn toàn bộ dịch vụ
    - Đặt càng gần **ĐÍCH** càng tốt
- **Extended ACL** :
    - Đề cập đến `source IP` , `destination IP` , `source port` , `destination port`  , `giao thức` ( TCP , UDP , ICMP ,... ) và 1 số thông tin khác của gói tin IP
    - Dải số từ :
        - `100` -> `199`
        - `2000` -> `2699`
    - Được lựa chọn dịch vụ muốn chặn
    - Đặt càng gần **NGUỒN** càng tốt
    
   <img src=https://i.imgur.com/mPz3W0O.png>  
## **3) Numbered ACL và Named ACL**
- Một **ACL** có thể định danh bởi số hiệu ( **Numbered** ) hoặc 1 chuỗi ký tự ( **Named** )
- Nguyên tắc hoạt động và cách sử dụng hoàn toàn giống nhau
- **Named ACL** cho phép ***chèn , sửa , xóa*** từng dòng .
- **Numbered ACL** thì không và phải viết lại toàn bộ nếu sai sót .
## **4) Cấu hình ACL**
### **4.1) Numbered ACL**
#### **4.1.1) Standard ACL**
- Cấu hình 1 **ACL** :
    ```
    Router(config) # access-list [number] [permit|deny] [source IP] [wildcard mask]
    Router(config) # access-list [number] [deny|permit] any
    ```

    **VD :**
    - **Deny** một dải mạng :
    ```
    Router(config) # access-list 1 deny 192.168.10.0 0.0.0.255
    Router(config) # access-list 1 permit any
    ```
    - **Deny** một host trong mạng :
    ```
    Router(config) # access-list 1 deny 192.168.10.2 0.0.0.0
    hoặc 
    Router(config) # access-list 1 deny host 192.168.10.2
    Router(config) # access-list 1 permit any
    ```
- Áp **ACL** lên 1 cổng :
    ```
    Router(config-if) # ip access-group [number] [in|out]
    ```
- Gỡ bỏ **ACL** :
    ```
    Router(config) # no access-list [number]
    Router(config) # interface [name]
    Router(config-if) # no ip access-group [number] [in|out]
    ```
- Kiểm soát **Telnet** :
    ```
    Router(config) # line vty 0 4
    Router(config-line) # access-class [number] [in|out]
    ```
    - **VD :** 
        ```
        R2(config) # access-list 10 deny 192.168.10.0 0.0.0.255
        R2(config) # access-list 10 permit any
        R2(config) # line vty 0 4
        R2(config-line) # access-class 10 in
        ```
- Kiểm tra **Access-list** :
    ```
    Router# show access-list
    ```
#### **4.1.1) Extended ACL**
- Cấu hình 1 **ACL** :
    ```
    Router(config) # access-list [number] [permit|deny] [protocol] [source IP] [wildcard mask] [operator port] [destination IP] [wildcard mask] [operator port]
    ```
    - Trong đó :
        - **Protocol** : **IP** , **ICMP** , **TCP** , **UDP** ,... ( chặn **IP** là chặn tất cả )
        - **Operator Port** : [ **lt** ( less than ) , **gt** ( greater than ) , **eq**( equal ) ] + [ services port ]
    - **VD :** 
        - **Deny** dịch vụ **HTTP** đến 1 Web Server `192.168.20.2` ( port `TCP/80` )
            ```
            Router(config) # access-list 100 deny tcp 192.168.10.0 0.0.0.255 host 192.168.20.2 eq 80        
            ( hoặc eq www )
            ```
        - **Deny** dịch vụ **TFTP** đến 1 TFTP Server `192.168.20.3` ( port  `UDP/69`)
            ```
            Router(config) # access-list 100 deny udp 192.168.10.0 0.0.0.255 host 192.168.20.3 eq 69
            ( hoặc eq tftp )
            ```
- Kiểm soát **Ping** :
    ```
    Router(config) # access-list [number] [deny|permit] icmp [source network|host] [des network|host]
    ```
### **4.2) Named ACL**
```
    Router(config) # ip access-list [standard|extended] [name]
    Router(config) # [permit|deny] . . .

    Router(config) # interface [name]
    Router(config-if) # ip access-group [name] [in|out]
```



