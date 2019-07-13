# Port Security
## **1) Đặc điểm**
- **Port Security** là tính năng Security Layer 2 trên Switch
- **Port Security** giúp phòng chống dạng tấn công phổ biến như giả MAC hay không cho người dùng hợp lệ truy cập vào mạng .
- **Port Security** cho phép nhận diện địa chỉ 1 port nào đó được gắn với MAC tương ứng và có hành vi xử lý khi rule bị vi phạm .
- Khi chỉ định địa chỉ source MAC đến 1 cổng bảo mật , cổng không chuyển tiếp các gói tin với địa chỉ nguồn bên ngoài nhóm của địa chỉ đã xác định .
- Không thể cấu hình **port-security** trên các port **trunk** .
- Không thể cấu hình **port-secutiry** trên destination port **SPAN** .
- Không thể cấu hình **port-security** trên các **port-channel** .

    <img src=https://i.imgur.com/OcVFrCv.jpg>
    
## **2) Cấu hình Port Security**
- Đưa port về mode **Access** ( bắt buộc ) :
    ```
    Switch(config-if) # switchport mode access
    ```
- Khởi động port-security :
    ```
    Switch(config-if) # switchport port-security
    ```
- Chỉ định số lần địa chỉ MAC được thay đổi :
    ```
    Switch(config-if) # switchport port-security maximum [number]
    ```
    => Đây là thông số chỉ định số lần tối đa mà port vẫn còn chấp nhận thay đổi địa chỉ MAC ,có nghĩa là thay đổi 1 host khác trong Switch
    
    Gọi số lần được phép thay đổi này là `k` , số lần thay đổi địa chỉ MAC là `n` . Port vẫn cho phép hoạt động nếu `n <= k` . Số lần thay đổi tối thiểu là `1` và tối đa là `1024`, mặc định là `1` .
- Chỉ định MAC cần được bảo mật trên interface :
    ```
    Switch(config-if) # switchport port-security mac-address [MAC]
    ```

    ***Chú ý*** : định dạng địa chỉ MAC là `AAAA.BBBB.CCCC` 
- Gán MAC tự động cho cổng :
    ```
    Switch(config-if) # switcport port-security mac-address sticky
    ```
- Chỉ định trạng thái của port khi thay đổi MAC kết nối bị sai :
    ```
    Switch(config-if) # switchport port-security violation [shutdown|restrict|protect]
    ```
    
    - Trong đó :
        - **shutdown** : port sẽ được đưa vào trạng thái lỗi và shutdown
        - **restrict** : port vẫn ở trạng thái ***up/up*** mặc dù địa chỉ kết nối bị sai tuy nhiên các gói tin đến port đều bị hủy ,và sẽ có 1 bản tin thông báo về số gói tin bị hủy
        - **protect** : port vẫn ***up/up*** như **restrict** , các gói tin đến port bị hủy và không có thông báo về việc hủy gói tin này
- Kiểm tra trạng thái **port-security** :
    ```
    Switch# debug port-security
    Switch# show port-security [address | interface [name] ]
    Switch# show interface [name]
    ```
