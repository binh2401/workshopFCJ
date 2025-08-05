---
title: "Dọn dẹp"
date: 2025-08-05
weight: 7
chapter: false
pre: "<b> 7. </b>"
---

## 🧨 Lưu ý trước khi xóa

- Luôn kiểm tra kỹ xem tài nguyên có đang được sử dụng không.
- Sao lưu dữ liệu quan trọng (nếu cần) trước khi thực hiện xóa.

---

## 🧹 1. Xóa VPC thủ công

### Bước 1: Truy cập VPC Dashboard  
- Mở: [https://console.aws.amazon.com/vpc](https://console.aws.amazon.com/vpc)  
- Vào mục **Your VPCs**

### Bước 2: Xóa các tài nguyên bên trong VPC:
- Subnets
- Route tables
- Internet gateways
- NAT gateways
- Security groups (tùy chỉnh)

### Bước 3: Xóa VPC
- Chọn VPC → Bấm **Actions** → **Delete VPC**

> ⚠️ Không thể xóa VPC nếu còn tài nguyên bên trong nó.

---

## 💾 2. Xóa RDS Instance thủ công

### Bước 1: Truy cập RDS Dashboard  
- Mở: [https://console.aws.amazon.com/rds](https://console.aws.amazon.com/rds)

### Bước 2: Vào mục **Databases**
- Click vào DB cần xóa → **Actions > Delete**

### Bước 3: Chọn có muốn giữ snapshot hay không
- Tick chọn `Skip final snapshot` nếu không cần lưu

---

## 🔐 3. Xóa Security Group

- Vào: **EC2 Dashboard** → **Security Groups**
- Chọn nhóm cần xóa → Bấm **Actions > Delete**

> Nhóm chỉ xóa được nếu không gán cho EC2 hay resource nào.

---

## 📊 4. Xóa Log Group trong CloudWatch

- Vào: [https://console.aws.amazon.com/cloudwatch](https://console.aws.amazon.com/cloudwatch)
- Chọn **Log groups** → tick vào log group → **Actions > Delete log group**

---

## 🔥 5. Xóa WAF Web ACL

- Vào: [https://console.aws.amazon.com/wafv2](https://console.aws.amazon.com/wafv2)
- Chọn Web ACL cần xóa
- Bấm **Delete**

> Nếu được gắn với ALB hay CloudFront thì phải tháo liên kết trước.

---

## 📁 6. Xóa Bucket S3

- Vào: [https://s3.console.aws.amazon.com/s3](https://s3.console.aws.amazon.com/s3)
- Click vào bucket → **Empty** để xóa toàn bộ file
- Sau đó **Delete bucket**

---

## ✅ Tổng kết

- Hãy xác nhận kỹ trước khi xóa.
- Ưu tiên sử dụng tagging để dễ tìm tài nguyên không dùng.
- Nên thiết lập cảnh báo (billing alerts) để tránh tốn chi phí do tài nguyên "quên xóa".
