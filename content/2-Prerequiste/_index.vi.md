---
title : "Các Bước Chuẩn Bị"
date :  "`r Sys.Date()`" 
weight : 2 
chapter : false
pre : " <b> 2. </b> "
---

{{% notice info %}}
Bạn cần chuẩn bị trước một số tài nguyên AWS và công nghệ lập trình để đảm bảo quá trình phát triển và triển khai hệ thống diễn ra suôn sẻ.
{{% /notice %}}

## 💻 Công nghệ sử dụng

| Công nghệ / Dịch vụ                  | Vai trò                                                                 |
|-------------------------------------|-------------------------------------------------------------------------|
| **AWS EC2**                         | Máy chủ triển khai ứng dụng web                                        |
| **AWS RDS**                         | Dịch vụ cơ sở dữ liệu (SQL Server) trên cloud                         |
| **AWS S3**                          | Lưu trữ hình ảnh bìa sách, tài liệu đính kèm                          |
| **AWS SNS**                         | Gửi thông báo email khi đơn hàng thay đổi trạng thái                   |
| **AWS IAM**                         | Quản lý phân quyền truy cập tài nguyên AWS                            |
| **AWS CloudWatch**                  | Ghi log hệ thống, theo dõi tình trạng server và hiệu suất             |
| **AWS WAF**                         | Bảo vệ website khỏi các mối đe dọa từ bên ngoài                       |
| **Graph Neural Networks + Neptune ML** | Gợi ý sách dựa trên mạng lưới tương tác người dùng – sách, đánh giá, tác giả, thể loại |

## 🔧 Các tài nguyên AWS cần chuẩn bị

Trước khi bắt đầu tích hợp các chức năng, bạn cần chuẩn bị:

- **1 EC2 instance (Linux hoặc Windows)** thuộc **public subnet** để triển khai ứng dụng web.
- **1 RDS instance (SQL Server)** để lưu dữ liệu sản phẩm, người dùng, đơn hàng.
- **1 IAM Role** có quyền truy cập SNS, S3, CloudWatch và các dịch vụ khác.
- **1 SNS Topic** đã đăng ký email nhận thông báo.
- **1 bucket S3** để lưu trữ ảnh bìa sách.
- **Cấu hình AWS WAF** để bảo vệ website khỏi các tấn công như SQLi, XSS, DDoS.
- **Cấu hình AWS CloudWatch** để theo dõi log hệ thống.
- **Tài khoản Neptune ML** để huấn luyện mô hình AI gợi ý sách (sử dụng GNN).

## 🔗 Tài liệu tham khảo

- [Giới thiệu về Amazon EC2](https://000004.awsstudygroup.com/vi/)
- [Làm việc với Amazon VPC](https://000003.awsstudygroup.com/vi/)
