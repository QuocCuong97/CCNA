# NAT - Network Address Translation
## **1) Kỹ thuật NAT là gì ?**
- Kỹ thuật **NAT** sẽ thực hiện chuyển đổi gói tin xuất phát từ vùng ***private*** thành địa chỉ IP ***public*** khi gói tin này đi từ mạng nội bộ ra mạng Internet và thực hiện chuyển đổi ngược lại các địa chỉ IP ***public*** thành ***private*** khi gói tin trả về từ Internet về mạng nội bộ .

    <img src=https://i.imgur.com/izj8i9t.png>

## **2) Một số khái niệm cơ bản**
- **Inside interface** : là cổng của Router biên nối xuống phần mạng ***prtivate*** .
- **Outside interface** : là cổng của Router biên nối đi mạng ***public*** - khu vực bên ngoài .
- **Địa chỉ Inside Local** : là các IP thuộc về mạng doanh nghiệp và đặt trên các host ở khu vực bên trong .
- **Địa chỉ Inside Global** : là các IP ***public*** thuộc về mạng doanh nghiệp được sử dụng để các host bên trong có thể thực hiện **NAT** đi Internet .
- **Địa chỉ Outside Global** : là các địa chỉ ***public*** trên Internet .
- **Địa chỉ Outside Local** : là các địa chỉ ***private*** nội bộ nhưng được sử dụng để đại diện cho các IP ***public*** bên ngoài .

    <img src=https://i.imgur.com/WIm1Lby.png>

## **3) Các kỹ thuật NAT**
### **3.1) NAT one-to-one**
- Phương thức này sẽ thực hiện **NAT** một địa chỉ bên trong thành 1 địa chỉ bên ngoài . Có bao nhiêu địa chỉ bên trong thì cần bấy nhiêu địa chỉ bên ngoài để đảm bảo tất cả các địa chỉ bên trong có thể đi được đến Internet 1 cách đồng thời .
- **NAT one-to-one** có 2 loại :
    - **Static NAT** : người quản trị phải cấu hình chỉ rõ địa chỉ ***private*** nào được **NAT** thành địa chỉ IP ***public*** nào . Các cặp địa chỉ được **NAT** với nhau sẽ được lưu trong bảng **NAT tĩnh** . Các gói tin đi Internet sẽ được Router **NAT** theo các cặp địa chỉ tĩnh đã được cấu hình .

        <img src=https://i.imgur.com/wsq80yQ.png>

    - **Dynamic NAT** : **NAT** được thực hiện 1 cách tự động . Trên Router , người quản trị cấu hình 1 danh sách các địa chỉ bên trong cần đi ra ngoài Internet và 1 danh sách các địa chỉ bên ngoài đại diện cho các địa chỉ bên trong . Tiếp theo , người quản trị cấu hình yêu cầu Router **NAT** danh sách bên trong thành danh sách bên ngoài . Bảng **NAT** của Router sẽ không có bất kỳ 1 dòng thông tin **NAT** nào được tạo sẵn mà các dòng thông tin **NAT** sẽ chỉ được tạo ra khi có gói tin đến Router ra Internet .

        <img src=https://i.imgur.com/EblCXX9.png>

### **3.2) NAT many-to-one**
- Còn được gọi là **NAT Overload** hay **PAT - Port Address Translation**
- **NAT Overload** cho phép **NAT** cùng lúc nhiều địa chỉ ***private*** bên trong thành 1 địa chỉ ***public*** bên ngoài .

    <img src=https://i.imgur.com/Xw3AuXx.png>
    
## **4) Cấu hình NAT**
### **4.1) Static NAT**
- Cấu hình 1 dòng **NAT** tĩnh : 
    ```
    Router(config) # ip nat inside source static [inside local] [inside global]
    ```
- Chỉ định các cổng **inside / outside** :
    ```
    Router(config-if) # ip nat [inside|outside]
    ```
### **4.2) Dynamic NAT** :
- Xác định dải địa chỉ đại diện bên ngoài ( ***public*** ) : các địa chỉ **NAT**
    ```
    Router(config) # ip nat pool [name] [start IP] [end IP] [netmask [subnet-mask]|prefix-length [number]]
    ```
- Thiết lập **ACL** cho phép những địa chỉ bên trong được chuyển đổi : các địa chỉ được **NAT** :
    ```
    Router(config) # access-list [number] permit [network] [subnet-mask]
    ```
- Thiết lập mối quan hệ giữa **ACL** và địa chỉ **NAT** :
    ```
    Router(config) # ip nat inside source list [ACL_number] pool [name]
    ```
- Xác định cổng **inside / outside** :
    ```
    Router(config-if) # ip nat inside | outside
    ```
### **4.3) NAT Overload**
- Xác định dãy địa chỉ cần chuyển đổi ra ngoài :
    ```
    Router(config) # access-list [number] permit [network] [subnet-mask]
    ```
- Cấu hình chuyển đổi địa chỉ IP sang cổng nối bên ngoài :
    ```
    Router(config) # ip nat inside source list [ACL number] interface [name] overload
    ```
- Xác định cổng **inside / outside** :
    ```
    Router(config-if) # ip nat inside
                      # ip nat outside
    ```
- Xóa bảng **NAT** :
    ```
    Router# clear ip nat translation
    ```
- Các lệnh kiểm tra cấu hình :
    ```
    Router# show ip nat translation
    Router# show ip nat statistics
    Router# debug ip nat
    ```
