# Khôi  phục mật khẩu cho Router ( ***enable password*** )
> ### **B1 : Tắt / Mở Router**
- Thực hiện tắt nguồn Router , chờ khoảng 15s rồi mở lại Router .

    ***Chú ý :*** Không được tắt mở quá nhanh để tránh sự cố về nguồn điện .
> ### **B2 : Sử dụng tổ hợp phím tắt để đi vào "rommon"**
- Trong quá trình Router khởi động lại , tiến hành sử dụng tổ hợp phím tắt `Ctrl` + `Break` ( `Break` = `Pause`) để đi vào mode **rommon**

    ![](/images/ccna/31_Khoi_phuc_mat_khau_cho_Router/Screenshot_1.png)
> ### **B3 : Đổi giá trị thanh ghi cấu hình**
- Thực hiện đổi giá trị thanh ghi cấu hình thành `0x2142` rồi khởi động lại . Với thanh ghi cấu hình `0x2142` , bit số 6 nhận giá trị là `1` .

    ![](/images/ccna/31_Khoi_phuc_mat_khau_cho_Router/Screenshot_1.png)
> ### **B4 : Sửa password**
- Thực hiện đi vào ***Privilledge mode*** và load lại cấu hình cũ để sửa pass .
    ```
    Router# copy startup-config running-config
    Router(config) # enable password [password]
    => Router# write
    ```
    ![](/images/ccna/31_Khoi_phuc_mat_khau_cho_Router/Screenshot_3.png)
> ### **B5 : Đổi giá trị thanh ghi cấu hình**
- Đổi lại giá trị thanh ghi cấu hình là `0x2102` rồi lưu và khởi động lại Router .
    ```
    Router(config) # config-register 0x2102
    Router(config) # end
    Router# write
    Router# reload
    ```
    ![](/images/ccna/31_Khoi_phuc_mat_khau_cho_Router/Screenshot_4.png)