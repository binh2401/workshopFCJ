---
title : "Tạo RDS database và triển khai web"
date :  "`r Sys.Date()`" 
weight : 4 
chapter : false
pre : " <b> 4. </b> "
---


# 1. Cài đặt Git trên Amazon EC2 (Amazon Linux 2023)

### Bước 1: Cập nhật hệ thống

```bash
sudo dnf update -y
sudo dnf search git
sudo dnf install git -y
git --version
```
![EC2](/images/4.s3/git.png)

### 2. Cài đặt asp.net trên Amazon EC2 Linux 2023 và clone file trên git

```bash
Cài đặt .NET SDK & Runtime
sudo dnf install -y dotnet-sdk-8.0
sudo dnf install -y aspnetcore-runtime-8.0
Kiểm tra: dotnet --version
```
+ sau khi kiểm tra version chúng ta bắt đầu  clone git 

```bash
git clone https://github.com/binh2401/FCJ.git

```

![EC2](/images/4.s3/moba1.png)




# Hướng dẫn tạo RDS SQL Server trên AWS

Amazon RDS hỗ trợ triển khai **Microsoft SQL Server** dưới dạng dịch vụ được quản lý. Hướng dẫn sau giúp bạn khởi tạo một SQL Server Instance nhanh chóng và an toàn.



## 🔧 Các bước thực hiện

### Bước 1: Mở Amazon RDS Console

- Truy cập: [https://console.aws.amazon.com/rds/](https://console.aws.amazon.com/rds/)
- Ở khu vực điều hướng trái → chọn **Databases**
![EC2](/images/4.s3/rdscr3.png)

- Nhấn **Create database**

---

### Bước 2: Chọn phương thức tạo

- Chọn **Standard create** (để tùy chỉnh được cấu hình)
- Engine: chọn **Microsoft SQL Server**

---

### Bước 3: Chọn phiên bản và edition

- **Edition**:
  - SQL Server Web Edition
  - SQL Server Express Edition
  - SQL Server Standard Edition
  - SQL Server Enterprise Edition (nếu cần các tính năng cao cấp)
- **Version**: Chọn phiên bản SQL Server (ví dụ: SQL Server 2019)

---

### Bước 4: Cấu hình thông tin đăng nhập

- `DB instance identifier`: tên định danh của DB
- `Master username`: ví dụ `admin`
- `Master password`: nhập mật khẩu và xác nhận

> 🔐 Nếu bỏ chọn “Auto generate a password” thì bạn cần nhập thủ công.

---

### Bước 5: Chọn lớp máy chủ và dung lượng lưu trữ

- **DB instance class**: ví dụ `db.t3.medium` (2 vCPU, 4 GB RAM)
- **Storage type**: chọn `General Purpose (SSD)` hoặc `Provisioned IOPS`
- Thiết lập dung lượng: ví dụ `20 GiB`

---

### Bước 6: Cấu hình mạng và bảo mật

- **Virtual Private Cloud (VPC)**: chọn VPC mặc định hoặc tùy chỉnh
- **Subnet group**: nếu chưa có, RDS sẽ tạo mới
- **Public access**: chọn `Yes` nếu muốn truy cập từ internet
- **VPC security group**:
  - Chọn `Create new` nếu chưa có
  - RDS sẽ tạo rule mở port 1433 cho IP hiện tại

---

### Bước 7: Thiết lập nâng cao (Optional)

- **Backup retention period**: giữ bản sao lưu bao nhiêu ngày (ví dụ 7)
- **Monitoring**: bật Enhanced monitoring nếu cần
- **Maintenance window**: thời gian bảo trì tự động
- **Deletion protection**: bảo vệ khỏi xóa nhầm

---

### Bước 8: Tạo SQL Server Instance

- Nhấn **Create database**
- Chờ vài phút đến khi trạng thái chuyển từ `Creating` sang `Available`

---

## 🔗 Kết nối tới RDS SQL Server hoặc chỉ cần kết nối tới data dẫn tới code trong appsettings.json

```json

  "ConnectionStrings": {
  "DefaultConnection": "của bạn,1433;Database=của bạn;User Id=admin;Password=của bạn;Encrypt=True;TrustServerCertificate=True"
}
```

Sau khi instance sẵn sàng:

- Truy cập tab **Connectivity & security**
- Ghi lại thông tin:
  - `Endpoint` (ví dụ: `mydb.abcxyz.rds.amazonaws.com`)
  - `Port`: 1433
  - `Username`: đã tạo trước đó

Bạn có thể kết nối bằng công cụ như:

```sql
-- Sử dụng SQL Server Management Studio (SSMS)
Server: <endpoint>,1433
Authentication: SQL Server Authentication
Username: <username>
Password: <password>
```
# vào EC2 để đẻ cài đặt sqlserver
## Cài đặt `mssql-tools` và `unixODBC` trên Amazon Linux 2023

Hướng dẫn cài đặt công cụ dòng lệnh `sqlcmd` và `bcp` từ Microsoft SQL Server trên Amazon EC2 dùng Amazon Linux 2023 (RHEL 9-based).

---

## Bước 1: Thêm kho Microsoft vào hệ thống

```bash
sudo rpm --import https://packages.microsoft.com/keys/microsoft.asc
sudo curl -o /etc/yum.repos.d/mssql-release.repo https://packages.microsoft.com/config/rhel/9/prod.repo
```
## bước 2: Cài đặt ODBC Driver và mssql-tools
```bash
sudo dnf update -y
sudo dnf install -y unixODBC unixODBC-devel
sudo ACCEPT_EULA=Y dnf install -y msodbcsql18 mssql-tools18
```
## bước 3: Thêm sqlcmd và bcp vào PATH
```bash
echo 'export PATH="$PATH:/opt/mssql-tools18/bin"' >> ~/.bashrc
source ~/.bashrc
```
## bước 4: Kiểm tra cài đặt
```bash 
sqlcmd -?
```
## 📊 Xem Logs và Events

- Truy cập tab **Logs & events**
- Có thể xem:
  - `Error log`
  - `SQL Server Agent log`
  - `Event log`

---

## 🛡 Xem bảo trì và sao lưu

- Vào tab **Maintenance & backups**
  - Kiểm tra lịch sao lưu tự động
  - Thực hiện snapshot thủ công
  - Theo dõi thời gian bảo trì

---

## ❗ Lưu ý quan trọng

- Không thể xem lại mật khẩu nếu chọn tự tạo. Nếu quên → cần sửa lại DB instance để reset.
- Cần mở port 1433 trong **Security Group** nếu truy cập từ bên ngoài.

---

📚 Tài liệu tham khảo:
- [Amazon RDS SQL Server Documentation](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/CHAP_SQLServer.html)



# Triển khai ASP.NET Core WebBanHang trên Amazon EC2 & RDS (Amazon Linux 2023)

## 1. Thiết lập hạ tầng AWS

- ✅ Tạo **VPC** riêng
- ✅ Tạo **Subnet** (Multi-AZ)
- ✅ Tạo **Security Group EC2**
- ✅ Tạo **Security Group RDS**
- ✅ Tạo **DB Subnet Group**
- ✅ Tạo **Amazon EC2 Instance**
- ✅ Tạo **Amazon RDS (SQL Server)**

---

## 2. Kết nối vào EC2 bằng SSH

**Sử dụng MobaXterm** hoặc dòng lệnh:

```bash
ssh -i "bookstore-key.pem" ec2-user@<EC2-PUBLIC-IP>
```

---

## 3. Cài đặt môi trường trên Amazon EC2 (Amazon Linux 2023)

### Cập nhật hệ thống:

```bash
sudo dnf update -y
```

### Cài Git:

```bash
sudo dnf search git
sudo dnf install git -y
git --version
```

### Cài .NET SDK & Runtime:

```bash
sudo dnf install -y dotnet-sdk-8.0
sudo dnf install -y aspnetcore-runtime-8.0
dotnet --version
```

---

## 4. Upload Project từ máy tính cá nhân lên EC2

Trên máy tính:

```powershell
scp -i "C:\Users\PC\Downloads\bookstore-key.pem" -r FCJ ec2-user@<EC2-PUBLIC-IP>:/home/ec2-user/
```

---

## 5. Publish ASP.NET Project

```bash
cd /home/ec2-user/FCJ/WebBanHang
dotnet publish -c Release -o out
ls out
```

---

## 6. Cấu hình Systemd Service

### Tạo file dịch vụ:

```bash
sudo nano /etc/systemd/system/webbanhang.service
```

### Nội dung:

```ini
[Unit]
Description=ASP.NET Core WebBanHang
After=network.target

[Service]
WorkingDirectory=/home/ec2-user/FCJ/WebBanHang/out
ExecStart=/usr/bin/dotnet /home/ec2-user/FCJ/WebBanHang/out/WebBanHang.dll
Restart=always
RestartSec=10
SyslogIdentifier=webbanhang
User=ec2-user
Environment=ASPNETCORE_ENVIRONMENT=Production
Environment=DOTNET_PRINT_TELEMETRY_MESSAGE=false

[Install]
WantedBy=multi-user.target
```

### Khởi động dịch vụ:

```bash
sudo systemctl daemon-reload
sudo systemctl enable webbanhang
sudo systemctl start webbanhang
sudo systemctl status webbanhang
```

---

## 7. Cài NGINX làm reverse proxy

```bash
sudo dnf install nginx -y
sudo systemctl enable nginx
sudo systemctl start nginx
```

### Cấu hình NGINX:

```bash
sudo nano /etc/nginx/conf.d/webbanhang.conf
```

```nginx
server {
    listen 80;
    server_name _;

    location / {
        proxy_pass         http://localhost:5000;
        proxy_http_version 1.1;
        proxy_set_header   Upgrade $http_upgrade;
        proxy_set_header   Connection keep-alive;
        proxy_set_header   Host $host;
        proxy_cache_bypass $http_upgrade;
        proxy_set_header   X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header   X-Forwarded-Proto $scheme;
    }
}
```

```bash
sudo systemctl restart nginx
```

---

## 8. Cài `sqlcmd` & kết nối RDS

### Cài công cụ:

```bash
sudo dnf install -y https://packages.microsoft.com/config/rhel/9/prod.repo
sudo dnf install -y mssql-tools unixODBC
echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
source ~/.bashrc
```

### Kết nối RDS:

```bash
sqlcmd -S <RDS-ENDPOINT> -U admin -P your_password
```

---

## 9. Apply Migration với EF Core

### Cài EF Tools:

```bash
export PATH="$PATH:$HOME/.dotnet/tools"
dotnet tool install --global dotnet-ef
```

### Apply database:

```bash
cd /home/ec2-user/FCJ/WebBanHang
dotnet ef database update --project WebBanHang.csproj
```

---

## 10. Cập nhật `appsettings.json`

```bash
cd /home/ec2-user/FCJ/WebBanHang/out
sudo nano appsettings.json
```

### Nội dung ví dụ:

```json
{
  "ConnectionStrings": {
    "DefaultConnection": "Server=book-store.cjyi2kukw3oj.ap-southeast-1.rds.amazonaws.com,1433;Database=store;User Id=admin;Password=Abc@123456;Encrypt=True;TrustServerCertificate=True"
  }
}
```

### Lưu lại:

```bash
Ctrl + O → Enter
Ctrl + X
```

---

## 11. Restart & Xem log dịch vụ

```bash
sudo systemctl restart webbanhang
journalctl -u webbanhang -n 50 --no-pager
```

---

## 🎉 DONE!

Mở trình duyệt:  
👉 `http://<EC2-PUBLIC-IP>/`


![EC2](/images/4.s3/image.png)

> ✅ Nếu không truy cập được, kiểm tra Security Group đã mở **port 80** chưa nhé.


---


