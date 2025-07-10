---
title : "Tạo VPC và chỉnh cấu hình"
date :  "`r Sys.Date()`" 
weight : 1 
chapter : false
pre : " <b> 2.1.1 </b> "
---


#### Tạo VPC 
1. Truy cập [giao diện quản trị dịch vụ VPC](https://console.aws.amazon.com/vpc/home)
  + Click **Your VPC**.
  + Click **Create VPC**.

![VPC](/images/2.prerequisite/001-createvpc.png)

2. Tại trang **Create VPC**.
  + Tại mục **Name tag** điền **Lab VPC**.
  + Tại mục **IPv4 CIDR** điền : **10.10.0.0/16**.

  ![VPC](/images/2.prerequisite/vpc1.png)

  + Click **Create VPC**.

![VPC](/images/2.prerequisite/vpc2.png)
  
  ## ℹ️ Thông tin

- **Mặc định**, subnet *không mặc định* có thuộc tính `Auto-assign public IPv4` là **false**.
- Subnet *mặc định* có thuộc tính này là **true**.
- Subnet *không mặc định* được tạo qua EC2 Launch Wizard sẽ mặc định là **true**.

---

## 🛠️ Các bước thay đổi cấu hình

### 1. Truy cập Amazon VPC Console

🔗 [Mở Amazon VPC Console](https://console.aws.amazon.com/vpc/)

### 2. Chọn subnet cần cấu hình

- Vào bảng điều hướng trái → **Subnets**.
![VPC](/images/2.prerequisite/subnet1.png)
- Chọn **subnet** bạn muốn cấu hình.

### 3. Chỉnh sửa subnet

- Nhấn **Actions → Edit subnet settings**.
![VPC](/images/2.prerequisite/subnet2.png)
- Bật hoặc tắt tùy chọn `Enable auto-assign public IPv4 address` theo nhu cầu.
![VPC](/images/2.prerequisite/subnet3.png)
- Nhấn **Save** để lưu thay đổi.
![VPC](/images/2.prerequisite/subnet4.png)

---

> ⚠️ **Lưu ý:** Thay đổi này **chỉ áp dụng cho các EC2 instance mới** được tạo trong subnet sau khi cấu hình. Những instance đang chạy sẽ **không bị ảnh hưởng**.

---

## ✅ Gợi ý

- Hãy đảm bảo subnet của bạn nằm trong VPC có Internet Gateway nếu muốn truy cập internet công khai.
- Kiểm tra lại security group và route table để đảm bảo kết nối.

