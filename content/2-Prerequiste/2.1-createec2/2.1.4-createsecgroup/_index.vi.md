---
title : "Tạo Security Group cho Amazon RDS"
date :  "`r Sys.Date()`" 
weight : 4
chapter : false
pre : " <b> 2.1.4 </b> "
---

# Tạo Security Group cho Amazon RDS

ℹ️ **Information:**  
Security Group cho Amazon RDS hoạt động như **tường lửa ảo** để kiểm soát lưu lượng mạng đến và đi từ cơ sở dữ liệu của bạn. Việc cấu hình đúng Security Group là bước quan trọng để bảo vệ dữ liệu của bạn trong AWS.

---

## 🛠️ Các bước tạo Security Group cho RDS

### 1. Đăng nhập vào AWS Management Console.

### 2. Truy cập dịch vụ **VPC**
- Vào menu dịch vụ.

- Chọn **VPC** trong phần **Networking & Content Delivery**.
![VPC](/images/2.prerequisite/rdssc1.png)

### 3. Mở danh sách **Security Groups**
- Trên thanh điều hướng bên trái, dưới mục **Security**, chọn **Security Groups**.
![VPC](/images/2.prerequisite/ec22.png)

### 4. Nhấp vào nút **Create security group** để bắt đầu quá trình tạo.

---
![VPC](/images/2.prerequisite/rdssc2.png)

## 📝 Cấu hình Basic Details

- **Security group name:** Nhập tên mô tả cho Security Group.
- **Description:** Thêm mô tả chi tiết về mục đích của Security Group.
- **VPC:** Chọn VPC nơi bạn sẽ triển khai cơ sở dữ liệu RDS.

---

## 🔐 Cấu hình Inbound Rules

1. Chọn **MSSQL** từ danh sách **Type**.
2. **Port 1433** sẽ được tự động điền.
3. **Source:** Chọn *Anywhere-IPv4** 

> 🔒 **Security Note:**  
> Chỉ cho phép kết nối từ các nguồn cụ thể thay vì mở cổng cơ sở dữ liệu cho tất cả địa chỉ IP (`0.0.0.0/0`). Điều này tuân theo nguyên tắc **đặc quyền tối thiểu** và tăng cường bảo mật.

---

## 📤 (Tùy chọn) Cấu hình Outbound Rules

- Mặc định, **tất cả lưu lượng đi ra đều được cho phép**.
- Bạn có thể giới hạn nếu cần bảo mật cao hơn.

---

## ✅ Tạo Security Group

- Sau khi hoàn tất cấu hình, nhấn nút **Create security group**.
- Security Group mới đã được tạo và sẵn sàng để gán cho **DB instance RDS** của bạn.

> ⚠️ **Warning:**  
> Không nên sử dụng **cùng một Security Group** cho cả **EC2** và **RDS**.  
> Việc **tách biệt Security Group** giúp quản lý quyền truy cập chính xác hơn và tuân thủ các **nguyên tắc bảo mật tốt nhất**.

---

## 💡 Pro Tip

Bạn có thể **chỉnh sửa quy tắc Security Group bất kỳ lúc nào**, và các thay đổi sẽ **được áp dụng ngay lập tức** cho tất cả tài nguyên liên kết với Security Group đó.