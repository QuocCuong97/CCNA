# VTP - VLAN Trunking Protocol
## **1) Đặc điểm**
- **VTP** sử dụng các đường **trunk** ***layer 2*** để trao đổi thông tin . Do đó , để các Switch chạy được **VTP** với nhau , các đường **trunk** phải được thiết lập giữa chúng .

- **VTP Domain** : các Switch chạy **VTP** với nhau được tổ chức thành các **domain** , chỉ các Switch thuộc cùng **domain** mới có thể chia sẻ thông tin với nhau . Mỗi **domain** được đặc trung bởi 1 **domain-name** , các Switch thuộc về cùng 1 **domain** phải được thiết lập **domain-name** giống nhau . **VTP domain-name** được xây dựng bởi các ký tự và chữ số , có phân biệt chữ hoa và chữ thường .
- **VTP Mode** : khi 1 Switch chạy **VTP** , nó có thể hoạt động ở các mode sau : 
    - **Server** : switch ở mode **server** có toàn quyền thao tác trên cấu hình **VLAN** . Các quyền này bao gồm : 
        - Tạo / sửa / xóa **VLAN**
        - Học cấu hình **VLAN** từ Switch khác ( thường được gọi là đồng bộ **VLAN** )
        - Forward thông tin **VLAN**
    - **Client** : Switch ở mode này không được phép thay đổi cấu hình **VLAN** . Các hành động mà Switch ở mode này thực hiện gồm :
        - Đồng bộ cấu hình **VLAN** từ các Switch khác
        - Forward thông tin **VLAN**
    - **Transparent** : Switch ở mode này giúp các Switch khác trao đổi thông tin **VTP** nhưng bản thân nó không đồng bộ cấu hình **VLAN** theo thông tin này .
    Các hành động mà 1 Switch ở mode này có thể làm được gồm :
        - Tạo / sửa / xóa **VLAN** độc lập với Switch khác
        - Không đồng bộ với cấu hình **VLAN** từ Switch khác cũng như không gửi cấu hình **VLAN** của mình cho các Switch khác
        - Forward thông tin **VLAN** đi qua nó

            <img src=https://i.imgur.com/ISZQDnm.png>
        
- Số **Revision** :
    - Mỗi Switch tham gia **VTP** sẽ duy trì 1 giá trị gọi là **revision** cho cấu hình **VLAN** đang lưu giữ .
    - Ở chế độ mặc định , số **revision** trên mỗi Switch bằng `0` , cứ mỗi lần Switch thực hiện tạo , sửa , xóa **VLAN** , giá trị này lại tăng lên 1 đơn vị .
    - Nếu 2 Switch cùng trao đổi cấu hình **VLAN** với nhau , cấu hình **VLAN** nào có số **revision** cao hơn sẽ đè lên cấu hình **VLAN** có số **revision** thấp hơn .
- **VTP prunning** : Tính năng này giúp Switch giảm thiểu việc phải forward những lưu lượng không cần thiết qua mạng chuyển mạch .
- **VTP version** :
    - Hiện có 3 **version** của **VTP** là **version** `1` , `2` , `3` , trong đó , 2 **version** được dùng phổ biến nhất là `1` và `2` .
    - Các Switch phải thống nhất với nhau về số **version VTP** được sử dụng là `1` hoặc `2` để có thể trao đổi thông tin với nhau vì 2 **version** này không tương thích ngược được với nhau . Switch chạy **Transparent version `2`** có thể forward thông tin cho Switch chạy **version** `1` .
    - **VTP version** `3` có thể tương thích ngược với **version** `2` và cung cấp thêm nhiều cải tiến mới trong hoạt động **VTP** .
## **2) Cấu hình VTP**
- Khai báo **domain-name** :
    ```
    Switch(config) # vtp domain [name]
    ```
- Khai báo password **VTP** :
    ```
    Switch(config) # vtp password [password]
    ```
- Chọn mode hoạt động cho Switch :
    ```
    Switch(config) # vtp mode [server|client|transparent]
    ```
- Bật **VTP prunning** :
    ```
    Switch(config) # vtp prunning
    ```
- Một vài lệnh kiểm tra :
    ```
    Switch # show vtp status
    Switch # show vtp password
    ```
