# Mạng máy tính
## 1) Mạng máy tính là gì?
- Mạng máy tinh là một hệ thống tập hợp bởi nhiều máy tính, laptop và các thiết bị được kết nối / liên kết với nhau bởi đường truyền vật lý theo một kiến trúc ***(Network Architecture)*** vào đó nhằm trao đổi dữ liệu và chia sẻ tài nguyên cho nhau.
- Phạm vi kết nối giữa các thiết bị hay các máy tính có thể nằm gói gọn trong 1 căn phòng,  1 tòa nhà lớn, hay cả 1 thành phố và thậm chí là trên toàn cầu.
- Tốc độ dữ liệu trên đường truyền còn được gọi là ***băng thông*** - thường được tính bằng số lượng bit được truyền đi trong 1 giây ( ***bps*** ).
## 2) Ứng dụng - chức năng chia sẻ tài nguyên
- Kết nối cho phép các thiết bị có thể chia sẻ dữ liệu, tài nguyên của cá nhân, tổ chức, các hoạt động chia sẻ thường thấy trong mạng doanh nghiệp:
    - Chia sẻ data và các loại application: người dùng có thể chia sẻ chung một hay nhiều loại tài nguyên với nhau như chia sẻ file giữa các máy tính kết nối với nhau, điều khiển truy nhập như Teamviewer hoặc thực hiện các video conference giữa các chi nhánh của công ty.
    - Trao đổi tài nguyên hardware như máy in, má**y photocopy,...
    - Share tài nguyên lưu trữ như: dữ liệu trong Server hay các hệ thống database, Data Center của các doanh nghiệp.
## 3) Các đặc tính kỹ thuật trong 1 mô hình mạng máy tính
- **Speed** : cho biết được tốc độ truyền tải dữ liệu được tính bằng đơn vị bps (bit per second).
- **Cost** : các chi phí xây dựng, lắp đặt, vận hành, bảo dưỡng 1 hệ thống mạng.
- **Security** : tính bảo mật dữ liệu trong hệ thống mạng
- **Availability** : tính liên tục trong việc đảm bảo truy cập liên tục.
- **Reliability** : độ tin cậy trên đường truyền, khả năng truyền dữ liệu ít mất mát và sự cố lỗi.
- **Topology** : sơ đồ mạng thực hiện cho người quản trị biết được cách thức kết nối giữa các thiết bị trong một hệ thống và cách thức các luồng dữ liệu được tải trong hệ thống.
## 4) Các thiết bị cơ bản của 1 hệ thống mạng
- **User đầu cuối (PC)** : các thiết bị dùng internet dùng mạng dây hay wifi để phục vụ truyền tải.
- **Các đường link kết nối**
    - Connector : cổng mạng RJ-45, RJ-11, ... phục vụ giao tiếp dữ liệu giữa đường truyền và NIC trên cac thiết bị.
    - Devices tiếp nhận dữ liệu : NIC - Network Interface Card ( card mạng có dây hoặc không dây ) thực hiện chuyển dữ liệu thành tín hiệu điện.
- **Những thiết bị tập trung** : **Switch**, **Hub**, **Bridge** có chức năng tập trung dữ liệu từ các end users. Thực hiện chuyển mạch dữ liệu ở Layer 2 Ethernet LAN.
- **Thiết bị định tuyến đường truyền** : **Router** : thực hiện chức năng định tuyến ( chọn đường đi tối ưu nhất ) cho dữ liệu tập trung ở Layer 2.
## 5) Các sơ đồ đấu nối trong 1 hệ thống
- Chia ra 2 dạng sơ đồ : 
    - **Physical Topology** : sơ đồ vật lý mô tả cách kết nối các thiết bị mạng.
    - **Logical Topology** : mô tả cách thức luồng dữ liệu được truyền tải.
- Các mô hình đấu nối cơ bản

    - ### **Bus**

        <img src=https://i.imgur.com/bS4hZlZ.jpg>

    - ### **Ring**

        <img src=https://i.imgur.com/xuVV3Ml.jpg>

    - ### **Star**

        <img src=https://i.imgur.com/QXc4HoO.jpg>

- Các mô hình chia theo mật độ kết nối 

   - ### **Full-mesh** : các thiết bị đều được kết nối đến các thiết bị còn lại. Tính dự phòng cao nhưng tốn chi phí đầu tư.

        <img src=https://i.imgur.com/MNLGhIs.jpg?1>

    - ### **Hub-and-Spoke** : kết nối thiết bị trung tâm đến các thiết bị còn lại. Sơ đồ này không có tính dự phòng, khi hub ( spoke ) chết thì các spke còn lại cô lập hoàn toàn, hệ thống mất tính dự phòng nhưng bù lại chi phí lắp đặt ít hơn Full-mesh.

        <img src=https://i.imgur.com/Tg9LzU0.jpg>

    - ### **Partial-mesh** : sự kết hợp giữa 2 sơ đồ trên, vừa đảm bảo được tính dự phòng vừa ít tốn chi phí cho việc vận hành và bảo dưỡng. Các thiệt bị đều có 1 đường dự phòng.

        <img src=https://i.imgur.com/LKPyTwx.jpg>

## 6) Các hình thức kết nối ra Internet
- ### **ADSL** ( Asymetric Digital Subcriber Line ) ###
    Là kỹ thuật sử dụng cáp đồng điện thoại để kết nối cung cấp đường truyền Internet. Thông qua đường cáp điện thoại này, các ISP sẽ cung cấp đường truyền đến điểm truy cập cho người dùng, thông qua đó user có thể truy cập vào Internet.
- ### **FTTH** ( Fiber to the Home ) và **FTTB** ( Fiber to the Bulding )
    Là kỹ thuật sử dụng đường cáp quang do ISP kéo đến nhà hay cơ quan để cung cấp Internet. Đường cáp FTTH và FTTB cung cấp đường truyền tốc độ cao hơn so với ADSL.
- ### **Cable TV**
    Là mạng lưới truyền hình cáp truy cập Internet. Được các nhà cung cấp cáp truyền hình cung cấp đường truyền Internet kèm theo dịch vụ cáp truyền hình
- ### **Leased-line**
    Một loại hình truy cập Internet dành cho các doanh nghiệp. Các ISP sẽ cung cấp đường truyền đảm bảo về chất lượng dịch vụ truy cập Internet.
## 7) **Quy mô của mạng**
- ### **Mạng cục bộ LAN ( Local Area Network )**
    - Có giới hạn về địa lý

    - Tốc độ truyền dữ liệu khá cao
    - Thường dùng MultiAccess Channel
    - Các kỹ thuật thường dùng: TokenRing - 16Mbps, Mạng hình sao

        <p align="center"><img src=https://i.imgur.com/M0A1n8F.png width=70%></p>

- ### **Mạng diện rộng WAN ( Wide Area Networks )**
    - Không có giới hạn về địa lý

    - Thường là sự kết nối nhiều LAN
    - Tốc độ truyền dữ liệu khá thấp
    - Do nhiều tổ chức quản lý
    - Thường dùng kỹ thuật Point-to-Point Channel
    - Các kỹ thuật thường dùng: các đường điện thoại và truyền thông vệ tinh

        <p align="center"><img src=https://i.imgur.com/6IEbwCj.png width=70%></p>

- ### **Mạng MAN ( Metropolis Area Network )**
    - Có kích thước vùng địa lý lớn hơn LAN nhưng nhỏ hơn WAN
    - Do 1 tổ chức quản lý
    - Thường dùng cáp đồng trục hay sóng ngắn
- ### **INTERNET**
    - Mạng toàn cầu đặc biệt kết nối mạng của các tổ chức, cá nhân trên toàn thế giới
    - Kết nối từ máy tính cá nhân đến Internet
    - Kết nối các LAN bởi WAN tạo nên Internet
- ### **INTRANET**
    - Là mạng LAN có triển khai các dịch vụ trên toàn Internet



