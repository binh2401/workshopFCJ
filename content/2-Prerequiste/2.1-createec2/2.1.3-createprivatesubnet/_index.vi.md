---
title : "Tạo EC2 Security Group"
date :  "`r Sys.Date()`" 
weight : 3
chapter : false
pre : " <b> 2.1.3 </b> "
---

## ℹ️ Security Group là gì?

**Security Group** hoạt động như một **tường lửa ảo**, giúp kiểm soát **lưu lượng truy cập vào và ra** khỏi các tài nguyên AWS như EC2.

- Mỗi Security Group chứa tập hợp các **quy tắc inbound (vào)** và **outbound (ra)**.
- Áp dụng cho **từng instance EC2** hoặc dịch vụ liên quan.

---

## 🛠️ Các bước tạo Security Group cho EC2

### Bước 1: Truy cập AWS EC2 Console

1. Đăng nhập vào [AWS Management Console](https://console.aws.amazon.com/).
2. Trong menu dịch vụ, chọn **EC2** trong mục **Compute**.
![VPC](/images/2.prerequisite/ec21.png)
---

### Bước 2: Mở mục Security Groups

- Trong thanh điều hướng bên trái, dưới **Network & Security** → chọn **Security Groups**.
![VPC](/images/2.prerequisite/ec22.png)
- Nhấn nút **Create Security Group**.

---

### Bước 3: Nhập thông tin cơ bản

- **Security group name**: Tên mô tả cho Security Group.
- **Description**: Mô tả chức năng của Security Group.
- **VPC**: Chọn VPC bạn muốn áp dụng.

![VPC](/images/2.prerequisite/ec22.png)

```txt
Ví dụ:
Name: web-server-sg
Description: Security group cho máy chủ web ứng dụng
VPC: vpc-0a1b2c3d
```

### 📥 Bước 4: Cấu hình Inbound Rules

Thêm các quy tắc để cho phép lưu lượng truy cập đến EC2 instance:

| **Loại lưu lượng** | **Cổng** | **Mô tả**                              |
|-------------------|----------|----------------------------------------|
| HTTP              | 80       | Truy cập web không bảo mật             |
| HTTPS             | 443      | Truy cập web bảo mật                   |
| Custom TCP        | 5000     | Cho phép ứng dụng chạy trên cổng 5000 |
| SSH               | 22       | Kết nối SSH để quản trị máy chủ        |



> 🔒 **Security Note:**  
> Trong môi trường sản xuất, **chỉ nên cho phép SSH từ các IP đáng tin cậy**, ví dụ: `203.0.113.0/24`  
> Tránh sử dụng `0.0.0.0/0` trừ khi bạn đang trong môi trường thử nghiệm hoặc đã kiểm soát bằng các lớp bảo mật khác.

---

### 📤 Bước 5: (Tuỳ chọn) Cấu hình Outbound Rules

- **Mặc định:** Cho phép tất cả lưu lượng đi ra.
- Bạn có thể giới hạn outbound nếu cần kiểm soát chặt hơn trong môi trường đặc biệt.

---
![VPC](/images/2.prerequisite/ec23.png)


### bước 6: Sau khi hoàn tất cấu hình, nhấp vào nút Create security group.

![VPC](/images/2.prerequisite/ec24.png)

> 💡 **Pro Tip:**  
> Bạn có thể **chỉnh sửa quy tắc Security Group bất kỳ lúc nào** và các thay đổi sẽ được **áp dụng ngay lập tức** cho tất cả các tài nguyên được liên kết với Security Group đó.

