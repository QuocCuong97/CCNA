# Static Route ( S )
> ## **1) Đặc điểm**
- Là phương pháp mà người quản trị phải cấu hình thủ công 
- Bắt buộc phải khai báo đích đến trong mạng 
- Bảo mật 
- Sử dụng phù hợp cho các hệ thống quy mô nhỏ
- **AD** = `0` ( connected interface ) hoặc **AD** = `1` ( remote route )
> ## **2) Ưu điểm và nhược điểm**
### **2.1) Ưu điểm**
- Sử dụng ít băng thông hơn định tuyến động
- Không tiêu tốn tài nguyên để tính toán và phân tích gói tin định tuyến
### **2.2) Nhược điểm**
- Không có khả năng tự cập nhật đường đi
- Phải cấu hình thủ công khi mạng có sự thay đổi
- Chỉ phù hợp với mạng nhỏ , rất khó khi triển khai trên mạng lớn
> ## **3) Các trường hợp bắt buộc phải dùng định tuyến tĩnh**
- Đường truyền có băng thông thấp
- Người quản trị mạng cần kiểm soát các kết nối
- Kết nối dùng định tuyến tĩnh là đường dự phòng cho đường kết nối dùng định tuyến động
- Chỉ có 1 đường duy nhất ra bên ngoài ( mạng stub )
- Router có ít tài nguyên và không thể chạy 1 giao thức định tuyến động
- Người quản trị viên cần kiểm soát bảng định tuyến và cho phép các giao thức **classful** và **classless**
> ## **4) Cấu trúc lệnh**
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

