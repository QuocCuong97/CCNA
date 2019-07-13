# RIP - Routing Information Protocol ( R ) 
## **RIPv2**
## **1) Đặc điểm**
- Thuộc dạng **Distance Vector**
- Sử dụng port `520 UDP`
- Giao thức **classless** ( `subnet-mask` đi kèm với `IP` )
- Hỗ trợ **VLSM** và **CIDR**
- **Metric** là `số hop count`
- Khả năng mở rộng thấp : số lượng `hop count` tối đa là `15` ; các tuyến đường vô hạn ( không thể truy cập ) có **metric** là `16`
- Cập nhật định kỳ mỗi `30s` đến địa chỉ **Multicast** `224.0.0.9`
- `25` tuyến đường cho mỗi tin nhắn **RIP** ( `24` nếu sử dụng xác thực)
- Hỗ trợ xác thực
- Thực thi **Split Horizon** và **Poison Reverse**
- Thực thi **Triggered Update**
- Giá trị **AD** cho **RIPv2** là `120`
- Không có khả năng mở rộng . Được sử dụng trong các mạng nhỏ , phẳng hoặc ở rìa các mạng lớn hơn .
## **2) Ưu điểm và nhược điểm**
### **2.1) Ưu điểm**
- Dễ dàng triển khai
- Không hạn chế khả năng thiết kế
- Sử dụng ít tài nguyên hệ thống
### **2.2) Nhược điểm**
- Tiêu tốn lượng lớn băng thông trong quá trình gởi bản tin **Broadcast** mỗi `30s/lần`
- Không có khả năng mở rộng vì giá trị cao nhất của `hop count ` = `15`
- Hội tụ mạng chậm
## **3) Các quy tắc chống loop**
- **Split-horizon** : Khi Router nhận được cập nhật định tuyến cho một subnet từ phía cổng nào thì nó sẽ không gửi ngược lại cập nhật này về phía cổng mà nó nhận được nữa .
    - **VD :** R2 sẽ không gửi lại thông tin mà nó học được từ R3 cho R3
- **Route-Poisoning** : Khi một subnet kết nối trực tiếp chuyển sang shutdown , Router sẽ gửi đi 1 bản tin cập nhật cho subnet này có **metric** = `16` ( *infinity metric* )
cho láng giềng của nó . Router láng giềng khi nhận được bản tin này sẽ cập nhật rằng subnet không còn nữa , đến lượt nó , nó lại tiếp tục phát ra 1 cập nhật định tuyến cho subnet này với **metric** = `16` ,... cứ thế , cả mạng sẽ nhanh chóng biết được subnet này không còn nữa .

- **Poison-Reverse** : Khi Router láng giềng nhận được bản tin update cho 1 subnet down có **metric** = `16` , nó cũng phải ngay lập tức hồi đáp về cho láng giềng một bản tin cập nhật cho subnet ấy cũng với **metric** = `16` . Hoạt động này được gọi là **Poison-Reverse**.
- **Trigger-Update** : việc phát ra các bản tin **Route-poisoning** và **Poison-Reverse** phải được thực hiện ngay lập tức mà không cần chờ tới hạn định kỳ gửi cập nhật định tuyến gọi là **Trigger-Update**
- **Holddown Timer** : Sau khi nhận được một poisoned route , Router sẽ khởi động bộ định thời **Holddown Timer** cho route này . Trước khi bộ Timer này hết hạn , không tin tưởng bất kỳ thông tin định tuyến nào khác , ngoại trừ thông tin đến từ chính láng giềng đã cập nhật cho mình route này đầu tiên . Giá trị default của **Holddown Timer** là `180s`.
## **4) Các Timer trong RIP**
- **Update Timer** : khoảng thời gian định kỳ gửi bản tin cập nhật định tuyến ra khỏi các cổng chạy **RIP** , giá trị default là `30s` . 
- **Invalid Timer** : khi Router đã nhận được cập nhật về 1 subnet nào đó mà sau khoảng thời gian **invalid timer** vẫn không nhận lại được cập nhật từ mạng này ( mà đúng ra là `30s/lần` ) , Router sẽ coi route đến subnet này là invalid nhưng vẫn chưa xóa khỏi bản định tuyến . Giá trị default của timer này là `180s` .
- **Flush Timer** : khi Router đã nhận được cập nhật về 1 subnet nào đó mà khoảng thời gian **flush timer** vẫn không nhận được cập nhật về mạng này ( mà đúng ra phải `30s/lần`  , Router sẽ xóa bỏ hẳn route này ra khỏi bảng định tuyến ) . Giá trị default của timer này là `240s` .

    <img src=https://i.imgur.com/YtbNpTo.png>
## **5) So sánh RIPv1 và RIPv2**
- **RIPv1** là giao thức **classful** trong khi **RIPv2** là giao thức **classless**
- **RIPv1** sử dụng địa chỉ **broadcast** `255.255.255.255` để gửi đi bản tin cập nhật trong khi **RIPv2** sử dụng địa chỉ **multicast** `224.0.0.9` để gửi đi các bản tin cập nhật .
- **RIPv1** không hỗ trợ xác thực trong định tuyến trong khi **RIPv2** có hỗ trợ xác thực
## **6) Cấu trúc lệnh**
```
Router(config) # router rip
Router(config-router) # version 2
Router(config-router) # network [connected network]
Router(config-router) # no auto-summary
Router(config-router) # exit
```
- Chứng thực trong **RIPv2**
    - Là cách thức bảo mật trong việc trao đổi thông tin định tuyến giữa các Router . Nếu có cấu hình chứng thực thì các Router phải vượt qua quá trình này trước khi việc thực hiện trao đổi thông tin định tuyến được thực hiện . **RIPv2** hỗ trợ 2 kiểu chứng thực là ***MD5*** và ***ClearText***
    - Chứng thực dạng ***ClearText*** : các Router được cấu hình 1 khóa ( password ) và trao đổi chúng để so khớp . Các khóa này được gửi dưới dạng không mã hóa trên đường truyền. Các bước cấu hình : 
        - **B1** : Tạo bộ khóa
        ```
        Router(config) # key chain [name]
        ```
        - **B2** : Tạo các khóa
        ```
        Router(config-keychain) # key [key id]
        Router(config-keychain-key) # key-string [password]
        ```
        - **B3** : Áp đặt vào cổng gửi chứng thực
        ```
        Router(config) # interface [name]
        Router(config-if) # ip rip authentication key-chain [name]
        ```
    - Chứng thực dạng ***MD5*** : dạng chứng thực này sẽ gửi thông tin về khóa đã được mã hóa giúp các thông tin trao đổi được an toàn hơn . Các bước cấu hình tương tự ***ClearText*** , chỉ khác bước 3 thêm lệnh sau :
        ```
        Router(config-if) # ip rip authentication mode md5
        ```
- Các lệnh kiểm tra cấu hình
    ```
    Router # show ip route
    ```
    hoặc
    ```
    Router # debug ip rip
    ```




