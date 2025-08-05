---
title : "Tạo kết nối đến máy chủ EC2"
date :  "`r Sys.Date()`" 
weight : 3 
chapter : false
pre : " <b> 3. </b> "
---

 Hướng Dẫn Tạo Amazon EC2 Instance

ℹ️ **Thông tin**: Amazon Elastic Compute Cloud (EC2) cung cấp khả năng tính toán có thể điều chỉnh quy mô trong đám mây AWS. Sử dụng EC2 cho phép bạn triển khai các máy chủ ảo mà không cần đầu tư vào phần cứng vật lý, giúp bạn phát triển và triển khai ứng dụng nhanh hơn.

---

## 📌 Các bước tạo EC2 Instance

### 1. Truy cập AWS Management Console
- Mở trình duyệt và truy cập vào Amazon EC2 console tại:  
  👉 https://console.aws.amazon.com/ec2/
![EC2](/images/2.prerequisite/ec2sv1.png)
---

### 2. Khởi tạo EC2 Instance
- Tại màn hình Dashboard của EC2 console, trong hộp **Launch instance**, chọn **Launch instance**.

![EC2](/images/2.prerequisite/ec2sv2.png)

---

## 1. Launch an Instance
![EC2](/images/2.prerequisite/ec2linux3.png)
### 🔖 Name and tags
- **Tên instance**: `book-store-server`

---

## 2. Application and OS Images (Amazon Machine Image)

### 🔹 Chế độ: `Quick Start`
- **Hệ điều hành**: Amazon Linux
- **Phiên bản**: `Amazon Linux 2023 AMI`
- **Chi tiết**:
  - Tên đầy đủ: `Amazon Linux 2023 AMI 2023.2.20240219.0 x86_64 HVM kernel-6.1`
  - Image ID: `ami-0c55b159cbfafe1f0`
  - Architecture: `x86_64`
  - Loại virtualization: `HVM`
  - Root device type: `EBS`
  - Platform: `Linux`
  - **Free tier eligible** ✅

---

## 3. Instance Type

- **Loại instance**: `t2.micro`
- 1 vCPU, 1 GiB RAM
- **Free tier eligible** ✅

---

## 4. Key Pair (Login)

- **Key pair đã chọn**: `book-store`
- Nếu chưa có thì ấn vào create key 

![EC2](/images/2.prerequisite/ec2linux1.png)
---

## 5. Network Settings

- **VPC**: `vpc-xxx (back-store-cloud-vpc)`
- **Subnet**: `subnet-xxx (back-store-cloud-public-sub-eastasia-b)`
- **Auto-assign public IP**: Enabled
- **Firewall (security group rules)**:
  - Chế độ: `Select existing security group`
  - **Security group**: `book-store-cloud-sg` (ID: `sg-034e625ff1fbdd04c`)
  - Cổng mở:
    - SSH (TCP port 22)
    - HTTP (TCP port 80)
    - HTTPS (TCP port 443)

---

## 6. Configure Storage

- **Ổ đĩa mặc định**:
  - Dung lượng: `10 GiB`
  - Loại ổ: `gp2 (General Purpose SSD)`
  - **Free tier eligible** ✅
- **Encrypted**: ❌ (Not encrypted)

---

## 7. Summary (Tóm tắt)

- **Name**: `book-store-server`
- **AMI**: Amazon Linux 2023
- **Instance type**: `t2.micro`
- **Key pair**: `book-store`
- **Security group**: `book-store-cloud-sg`
- **Storage**: 10 GiB gp2
- **Free tier eligible**: ✅

---

## ✅ Sau khi hoàn tất:

1. Nhấn nút **Launch instance**.
![EC2](/images/2.prerequisite/ec2linux4.png)
2. Chờ trạng thái instance chuyển từ `pending` sang `running`.
3. Kết nối SSH với instance bằng IP công khai hoặc DNS thông qua MobaXterm hoặc terminal.

# Kết nối vào EC2 Instance bằng SSH sử dụng MobaXterm

ℹ️ **Thông tin**:  
MobaXterm là một ứng dụng đa năng cho Windows cung cấp nhiều công cụ mạng trong một giao diện duy nhất, bao gồm:
- SSH client
- SFTP
- X11 server
- Và nhiều tính năng khác

---

## 🧰 Bước 1: Tải và cài đặt MobaXterm

- Tải MobaXterm từ trang chính thức: 👉 [MobaXterm Website](https://mobaxterm.mobatek.net/)
- Cài đặt ứng dụng theo hướng dẫn.

---

## 🔌 Bước 2: Tạo phiên SSH mới

1. Khởi động **MobaXterm**.
2. Nhấp vào biểu tượng **Session** ở góc trên bên trái.

---

## ⚙️ Bước 3: Cấu hình kết nối SSH

Trong cửa sổ cấu hình:
- **Remote Host**: Nhập địa chỉ IP công khai hoặc DNS công khai của EC2 instance.
- **Port**: `22` (cổng SSH mặc định)
- **User**: `ec2-user` (cho Amazon Linux)

### ➕ Advanced SSH settings:
- Chọn tab này để chỉ định đường dẫn đến **tệp khóa riêng tư** (`.pem`) mà bạn dùng để kết nối SSH.

🔒 **Ghi chú bảo mật**:

- Đảm bảo file `.pem` có quyền hạn chế (chỉ user hiện tại có thể đọc).
- Trên Linux/Mac, dùng lệnh: `chmod 400 file.pem`

---

## 🖥️ Bước 4: Kết nối vào EC2 Instance

- Nhấn **OK** để lưu và bắt đầu phiên SSH.
- Nếu đây là lần đầu kết nối:
  - Sẽ hiện cảnh báo về **host key không xác định**.
  - Chọn **Yes** để tiếp tục.

---

## ✅ Xác nhận kết nối thành công

- Sau khi kết nối, bạn sẽ thấy **terminal của EC2 instance**.
- Bạn có thể bắt đầu chạy các lệnh như: `sudo yum update`, `top`, `htop`,...
![EC2](/images/3.connect/ec2linuxconet1.png)

💡 **Mẹo nhỏ**:
- MobaXterm tự động lưu các phiên gần đây.
- Giúp bạn dễ dàng kết nối lại mà không cần cấu hình lại từ đầu.

---

**🎉 Hoàn tất!** Bạn đã kết nối SSH vào EC2 instance thành công.
