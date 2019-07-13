# Default Route ( S* )
## **1) Đặc điểm**
- Sử dụng trong trường hợp không biết được đích đến của gói tin , thường được sử dụng trong hệ thống Internet khi mà không cần biết đích đến
- Được sử dụng tại các vị trí cuối trong hệ thống mạng
- Là tuyến đường được ưu tiên cuối cùng trong bảng định tuyến
- Có thể giúp cho việc làm giảm lượng thông tin mà bảng định tuyến cần phải có
## **2) Cấu trúc lệnh**
```
Router(config) # ip route 0.0.0.0 0.0.0.0 [IP next hop / exit interface]
```