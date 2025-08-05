---
title : Tổng Quan và Cấu Hình AWS WAF ,cloudfront"
date :  "`r Sys.Date()`" 
weight : 5 
chapter : false
pre : " <b> 5. </b> "
---

# 🔐 I. Tổng quan AWS WAF

**AWS WAF (Web Application Firewall)** là dịch vụ tường lửa ứng dụng web giúp bảo vệ website hoặc API khỏi các mối đe dọa phổ biến như:

- SQL Injection
- Cross-Site Scripting (XSS)
- Tấn công DDoS (khi kết hợp với **AWS Shield**)
- Các request bất thường (dựa trên IP, quốc gia, header,...)

---

## 🛠️ II. Các bước cấu hình AWS WAF
#### 1. Tạo Application Load Balancer (ALB)
- Vào **EC2 Console** → **Load Balancers** → **Create Load Balancer**
![FWD](/images/5.fwd/waf3.png)
- Chọn loại: **Application Load Balancer**
- Cấu hình:
  - Đặt tên ALB
  - Chọn schema: Internet-facing (nếu website public)
  - Chọn VPC, Subnet
  - Cấu hình security group mở port 80/443
![FWD](/images/5.fwd/waf4.png)

####  Tạo Target Group
- Trong quá trình tạo ALB, chọn **Create a new target group**
![FWD](/images/5.fwd/waf8.png)
- Loại target: **Instance**
- Chọn protocol: **HTTP hoặc HTTPS**
- Thêm các **EC2 instances** đang chạy website vào target group
![FWD](/images/5.fwd/waf5.png)
![FWD](/images/5.fwd/waf6.png)
![FWD](/images/5.fwd/waf7.png)

![FWD](/images/5.fwd/waf9.png)
![FWD](/images/5.fwd/waf10.png)
![FWD](/images/5.fwd/waf11.png)
#### 3. Gắn AWS WAF vào ALB
- Truy cập **AWS WAF & Shield**
- Chọn **Web ACLs** → **Create Web ACL**
- Tại bước **Association**, chọn ALB bạn vừa tạo

### 1. Tạo Web ACL (Access Control List)
- Truy cập **AWS Console** → Tìm **WAF & Shield**
![FWD](/images/5.fwd/image.png)
- Chọn **Web ACLs** → **Create web ACL**
![FWD](/images/5.fwd/waf1.png)
- Đặt tên, chọn **Region** nếu sử dụng **ALB**, hoặc **CloudFront** nếu sử dụng CDN.
![FWD](/images/5.fwd/waf2.png)

![FWD](/images/5.fwd/waf12.png)
### 2. Chọn tài nguyên cần bảo vệ
- Gắn Web ACL với một trong các dịch vụ:
  - **Application Load Balancer (ALB)**
  - **Amazon API Gateway**
  - **Amazon CloudFront** (Khuyến khích nếu website phục vụ toàn cầu)

### 3. Thêm các Rule vào Web ACL

Bạn có thể chọn:

#### ✅ Managed Rule Groups của AWS:
![FWD](/images/5.fwd/waf13.png)

![FWD](/images/5.fwd/waf14.png)

### 4. Cấu hình hành động cho mỗi Rule
- `Allow`: Cho phép
- `Block`: Chặn
- `Count`: Ghi nhận nhưng không chặn (giúp kiểm thử trước khi áp dụng thực tế)

### 5. Gắn Web ACL với Resource
Sau khi tạo Web ACL, bạn gắn nó với tài nguyên cần bảo vệ:


Client → CloudFront (gắn AWS WAF) → ALB → EC2/Laragon (.NET App)
![FWD](/images/5.fwd/waf15.png)
![FWD](/images/5.fwd/waf16.png)

# Tích hợp AWS CloudWatch vào ứng dụng ASP.NET MVC
## ✅ Mục tiêu

- Ghi log lỗi, log hệ thống từ **ASP.NET MVC** lên **AWS CloudWatch Logs**
- Tự động tạo Log Group/Stream nếu chưa có
- Không cần cài CloudWatch Agent

---

## 🧰 Yêu cầu

- Ứng dụng ASP.NET MVC (Core hoặc .NET Framework)
- Tài khoản AWS và IAM user có quyền ghi CloudWatch Logs

---

## 🔧 Bước 1: Cài đặt gói AWS Logger

Mở terminal hoặc Package Manager Console và chạy:

```bash
dotnet add package AWS.Logger.AspNetCore
```

> Nếu bạn dùng .NET Framework MVC cũ, thay vào đó dùng:  
> `AWS.Logger.Log4Net` hoặc `AWS.Logger.NLog` tuỳ theo hệ thống logging bạn sử dụng.

---

## ⚙️ Bước 2: Cấu hình `appsettings.json`

```json
{
  "AWS": {
    "Region": "ap-southeast-1",
    "LogGroup": "MvcAppLogGroup"
  },
  "Logging": {
    "LogLevel": {
      "Default": "Information",
      "Microsoft": "Warning"
    }
  }
}
```

---

## 🧬 Bước 3: Cấu hình logger trong mã nguồn

### ✅ Đối với ASP.NET Core MVC (.NET 6 trở lên):

Sửa file `Program.cs`:

```csharp
using AWS.Logger.AspNetCore;

var builder = WebApplication.CreateBuilder(args);

builder.Logging.AddAWSProvider(builder.Configuration.GetAWSLoggingConfigSection());

builder.Services.AddControllersWithViews();
var app = builder.Build();

app.UseRouting();
app.UseAuthorization();
app.MapDefaultControllerRoute();
app.Run();
```

---

## 🔐 Bước 4: Cấu hình quyền truy cập AWS

### 👉 Nếu chạy trên EC2:

Gán IAM Role cho EC2 instance với quyền:

```json
{
  "Effect": "Allow",
  "Action": [
    "logs:CreateLogGroup",
    "logs:CreateLogStream",
    "logs:PutLogEvents"
  ],
  "Resource": "*"
}
```

### 👉 Nếu chạy local (dev):

Tạo file `~/.aws/credentials` hoặc `C:\Users\<tên user>\.aws\credentials`:

```ini
[default]
aws_access_key_id = YOUR_ACCESS_KEY
aws_secret_access_key = YOUR_SECRET_KEY
```

---

## 🧪 Bước 5: Thêm dòng ghi log trong controller

```csharp
private readonly ILogger<HomeController> _logger;

public HomeController(ILogger<HomeController> logger)
{
    _logger = logger;
}

public IActionResult Index()
{
    _logger.LogInformation("Trang chủ được truy cập lúc " + DateTime.Now);
    return View();
}
```

---

## 📊 Bước 6: Kiểm tra log trên AWS

1. Truy cập **AWS Console > CloudWatch > Log groups**
2. Chọn `MvcAppLogGroup`
3. Chọn log stream để xem chi tiết

---

## 🧩 Ghi chú thêm

- Bạn có thể dùng thêm `AWS X-Ray` để trace hiệu năng request nếu ứng dụng phức tạp.
- Có thể tích hợp `LogLevel`, `EventID`, `Exception` để có log chi tiết hơn.

---

**Chúc bạn triển khai thành công!** Nếu bạn cần bản mẫu cho .NET Framework MVC hoặc muốn cấu hình ghi log nâng cao, hãy để lại bình luận hoặc liên hệ.

