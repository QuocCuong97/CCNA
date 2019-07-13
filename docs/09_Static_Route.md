# Static Route ( S )
## **1) Đặc điểm**
- Là phương pháp mà người quản trị phải cấu hình thủ công 
- Bắt buộc phải khai báo đích đến trong mạng 
- Bảo mật 
- Sử dụng phù hợp cho các hệ thống quy mô nhỏ
- **AD** = `0` ( connected interface ) hoặc **AD** = `1` ( remote route )
## **2) Ưu điểm và nhược điểm**
### **2.1) Ưu điểm**
- Sử dụng ít băng thông hơn định tuyến động
- Không tiêu tốn tài nguyên để tính toán và phân tích gói tin định tuyến
### **2.2) Nhược điểm**
- Không có khả năng tự cập nhật đường đi
- Phải cấu hình thủ công khi mạng có sự thay đổi
- Chỉ phù hợp với mạng nhỏ , rất khó khi triển khai trên mạng lớn
## **3) Các trường hợp bắt buộc phải dùng định tuyến tĩnh**
- Đường truyền có băng thông thấp
- Người quản trị mạng cần kiểm soát các kết nối
- Kết nối dùng định tuyến tĩnh là đường dự phòng cho đường kết nối dùng định tuyến động
- Chỉ có 1 đường duy nhất ra bên ngoài ( mạng stub )
- Router có ít tài nguyên và không thể chạy 1 giao thức định tuyến động
- Người quản trị viên cần kiểm soát bảng định tuyến và cho phép các giao thức **classful** và **classless**
## **4) Bảng định tuyến - Routing Table**
- Một bảng định tuyến gồm nhiều **entry** , mỗi **entry** chứa thông tin về tuyến đường đến các đích khác nhau. Cấu trúc 1 entry bao gồm : 
    - **Destination IP** : địa chỉ này có thể là địa chỉ của 1 host cụ thể , hoặc là 1 địa chỉ của 1 mạng .
    - **IP của next-hop Router , hoặc địa chỉ của một mạng kết nối trực tiếp (directly connected IP address)** : là địa chỉ của đích đến tiếp theo (Router) có thể chuyển tiếp gói tin đến đích .
    - **Network Interface** : là cổng của Router được sử dụng để gửi gói tin đến next-hop .
    - **Cờ (flags)** : cho biết nguồn cập nhật của tuyến route . VD : S - Static Route , C - Connected Route , O - OSPF Route ,...
    - **Metric** : là thông tin về metric của 1 tuyến đường , thể hiện "khoảng cách" từ Router hiện tại đến Destination IP . Giá trị này chỉ có ý nghĩa so sánh khi các route sử dụng cùng 1 giao thức định tuyến .
    - **Administrative Distance (AD)** : tham số ưu tiên mà người quản trị đặt cho các tuyến đường trong bảng định tuyến , được gán cho các giao thức . Nếu tuyến đường được cập nhật từ giao thức , nó sẽ mang giá trị AD của giao thức đó . Giá trị này nằm trong khoảng từ 0 đến 255 , càng bé càng ưu tiên ,255 nghĩa là không bao giờ được sử dụng . 
    
        <img src=https://i.imgur.com/ieOdQly.png>


## **5) Cấu trúc lệnh**
```
Router(config) # ip route [des-neswork] [des-subnet-mask] [IP next hop / exit interface] [AD]
```
=> **AD** sử dụng trong trường hợp có cấu hình dự phòng , **AD** càng nhỏ tuyến đường càng ưu tiên . 
- Kiểm tra bảng định tuyến
    ```
    Router # show ip route
    ```
    hoặc
    ```
    Router # show ip route static
    ```

