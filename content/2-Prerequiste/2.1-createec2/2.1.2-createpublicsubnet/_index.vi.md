---
title : "Tạo Public subnet"
date :  "`r Sys.Date()`" 
weight : 2
chapter : false
pre : " <b> 2.1.2 </b> "
---

## ℹ️ DB Subnet Group là gì?

**DB Subnet Group** là tập hợp các subnet trong VPC được chỉ định cho cơ sở dữ liệu Amazon RDS. Việc tạo DB Subnet Group giúp RDS có thể triển khai các instance cơ sở dữ liệu trong **nhiều Availability Zone (AZ)** nhằm đảm bảo:

- Tính **sẵn sàng cao**
- Khả năng **chịu lỗi**

---

## 🛠️ Các bước tạo DB Subnet Group

### Bước 1: Truy cập Amazon RDS

1. Đăng nhập vào [AWS Management Console](https://console.aws.amazon.com/).
2. Trong menu dịch vụ, tìm và chọn **Amazon RDS**.
![VPC](/images/2.prerequisite/rds1.png)

### Bước 2: Mở giao diện Subnet Groups

- Trong thanh điều hướng bên trái → chọn **Subnet groups**

- Nhấn nút **Create DB Subnet Group**
![VPC](/images/2.prerequisite/rds2.png)

---

### Bước 3: Nhập thông tin cơ bản

- **Name**: Tên mô tả cho DB Subnet Group
- **Description**: Mô tả chi tiết
- **VPC**: Chọn VPC nơi bạn sẽ triển khai RDS

![VPC](/images/2.prerequisite/rds3.png)

```txt
Ví dụ:
Name: book-store-subnet-group
Description: book-store-subnet-group
VPC: vpc-0a1b2c3d
```
- Sau khi xong nhấn **Nhấn create**: 
![VPC](/images/2.prerequisite/rds4.png)

