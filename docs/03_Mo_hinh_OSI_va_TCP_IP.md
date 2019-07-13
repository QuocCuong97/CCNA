# Mô hình OSI và TCP/IP
## **1) Mô hình OSI**
### **1.1) Giới thiệu**
- Được giới thiệu vào năm 1977 do tổ chức ISO. Thực hiện các tác vụ truyền dẫn số liệu, dữ liệu giữa 2 host thành 7 layer từ 1 -> 7
### **1.2) Các phân lớp mô hình OSI**

<img src=https://i.imgur.com/lXntthS.png>

- **Physical Layer (Layer 1)** : chuyển đổi các dữ liệu thành các tín hiệu cơ, điện, quang từ các bit nhị phân (0,1) để truyền trên đường truyền vật lý.

- **Data-Link Layer (Layer 2)** : có chức năng định nghĩa các cách thức đóng gói dữ liệu cho các loại đường truyền. Thực hiện tương tác với các giao thức của lớp trên, tầng Data Link sử dụng **địa chỉ MAC** là địa chỉ đặc trung cho tầng này. **Switch** là thiết bị hoạt động ở tầng Data-Link.

- **Network Layer (Layer 3)** : vai trò của tầng Network là định tuyến đường truyền, tìm ra đường đi tối ưu nhất cho các gói tin. **Địa chỉ IP** được sử dụng phổ biến ở Layer 3. **Router** là thiết bị hoạt động đặc trưng ở tầng này.

- **Transport Layer (Layer 4)** : làm công việc quản lý, thực hiện các tác vụ truyền dữ liệu từ source đến destination. Đảm bảo việc truyền dữ liệu được tối ưu tốt nhất, lưu ý là các tác vụ truyền này phải đảm bảo thông suốt từ Layer 1, 2, 3.

- **Session Layer (Layer 5)** : có vai trò chính trong việc thiết lập, duy trì, và giải phóng các session trao đổi dữ liệu giữa các ứng dụng trên 2 hosts.

- **Presentation Layer (Layer 6)** : thực hiện translate 2 ứng dụng giữa 2 host với nhau để 2 host này có thể hiểu và giao tiếp được với nhau.

- **Application Layer (Layer 7)** : giao diện tương tác trực tiếp giữa các ứng dụng và dịch vụ mạng đến người dùng.
## 2) **Mô hình TCP/IP**
- Mô hình này cũng được sử dụng khá rộng rãi, độ phổ biến tương đương mô hình OSI.

    <img src=https://i.imgur.com/oDHWrEA.png>

- Khác với OSI, TCP/IP tổ chức dữ liệu theo 4 tầng:

    - **Network Access Layer (Layer 1)** : có vai trò của tầng Data Link và Physical trong mô hình OSI.

    - **Internet Layer (Layer 2)** : Hoạt động như tầng Network OSI. Đặc trưng được sử dụng rộng rãi của tầng này là địa chỉ IP.

    - **Transport Layer (Layer 3)** : tương đương Transport của mô hình OSI. Giao thức nổi tiếng của tầng này là **TCP** và **UDP**.

    - **Application Layer (Layer 4)** : kiêm luôn nhiệm vụ tần 5, 6, 7 của OSI.

- ### **Mô hình TCP/IP 5 Layers**

    <img src=https://i.imgur.com/wMXLKUq.jpg>

## 3) **So sánh Mô hình OSI - TCP/IP**
<img src=https://i.imgur.com/eRlr5qd.png>