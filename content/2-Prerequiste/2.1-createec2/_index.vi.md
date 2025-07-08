---
title : "Chuẩn bị VPC và EC2"
date :  "`r Sys.Date()`" 
weight : 1 
chapter : false
pre : " <b> 2.1 </b> "
---

## 🎯 Mục tiêu

Trong bước này, chúng ta sẽ thiết lập hạ tầng mạng cơ bản gồm:

- Một **Virtual Private Cloud (VPC)** riêng biệt cho toàn bộ hệ thống.
- Hai **subnet**:
  - `Public subnet`: Dùng để triển khai các EC2 instance truy cập được từ Internet (máy chủ Linux).
  - `Private subnet`: Dùng để triển khai các instance hoặc dịch vụ chỉ truy cập nội bộ (máy chủ Windows, RDS, Neptune…).
- Một **security group** để mở port kết nối phù hợp.
- Hai **EC2 Instance**:
  - Một máy chủ **Linux** triển khai ứng dụng ASP.NET MVC (nếu dùng container hoặc reverse proxy).
  - Một máy chủ **Windows** trong private subnet để kết nối nội bộ, kiểm thử, chạy cơ sở dữ liệu nội bộ hoặc quản trị.

---

## 🗂️ Kiến trúc tổng quan

Sau khi hoàn tất các bước cấu hình, kiến trúc cơ bản sẽ như hình minh họa sau:

![VPC](/images/arc-01.png)

---

## 📘 Tài liệu tham khảo

Bạn có thể tham khảo thêm các bài hướng dẫn chuyên sâu sau để hiểu rõ hơn:

- [Giới thiệu về Amazon EC2](https://000004.awsstudygroup.com/vi/)
- [Làm việc với Amazon VPC](https://000003.awsstudygroup.com/vi/) 

---

## 📋 Nội dung từng bước

| STT | Mô tả | Đường dẫn |
|-----|-------|-----------|
| 1 | Tạo VPC | [2.1.1 - Tạo VPC](2.1.1-createvpc/) |
| 2 | Tạo subnet công khai (Public Subnet) | [2.1.2 - Tạo Public Subnet](2.1.2-createpublicsubnet/) |
| 3 | Tạo subnet riêng (Private Subnet) | [2.1.3 - Tạo Private Subnet](2.1.3-createprivatesubnet/) |
| 4 | Tạo Security Group mở port phù hợp | [2.1.4 - Tạo Security Group](2.1.4-createsecgroup/) |
| 5 | Tạo EC2 Instance Linux trong Public Subnet | [2.1.5 - Tạo EC2 Linux](2.1.5-createec2linux/) |
| 6 | Tạo EC2 Instance Windows trong Private Subnet | [2.1.6 - Tạo EC2 Windows](2.1.6-createec2windows/) |

---

## 🔧 Các bước tiếp theo sau khi hoàn tất

Sau khi triển khai VPC và EC2, bạn có thể tiếp tục các bước sau:

- Tạo **IAM Role** cấp quyền cho EC2 sử dụng SNS, CloudWatch, SSM...
- Tạo **SNS Topic** để gửi email.
- Cấu hình **CloudWatch Logs**.
- Tạo **RDS Instance** nếu cần cơ sở dữ liệu riêng.
- Tạo **Neptune Cluster** nếu dùng AI Recommendation.
