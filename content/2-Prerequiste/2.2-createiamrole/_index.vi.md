---
title : "Tạo SNS và IAM cho gửi Email"
date :  "`r Sys.Date()`" 
weight : 3
chapter : false
pre : " <b> 2.3 </b> "
---

### 📬 Mục tiêu

Trong bước này, chúng ta sẽ thực hiện:

- Tạo một **SNS Topic** để gửi email khi đơn hàng đặt thành công.
- Tạo **IAM Policy** cho phép ứng dụng gửi thông báo thông qua SNS.
- Gán quyền đó cho EC2 Instance hoặc dịch vụ backend.

---

## 🛠️ Tạo SNS Topic

1. Truy cập vào dịch vụ [Amazon SNS](https://console.aws.amazon.com/sns/v3/home).
![Tên ảnh hiển thị](/images/2.prerequisite/image.png)


2. Chọn **Topics** từ thanh menu bên trái, sau đó click **Create topic**.

![Tên ảnh hiển thị](/images/2.prerequisite/image2.png)

3. Chọn loại topic là **Standard**.
4. Nhập tên cho topic, ví dụ: `book-email`.
![Tên ảnh hiển thị](/images/2.prerequisite/image3.png)

5. Giữ nguyên các cài đặt mặc định và click **Create topic**.

---

## 📥 Đăng ký email nhận thông báo (Subscription)

1. Sau khi tạo topic, chọn topic vừa tạo trong danh sách.
2. Trong tab **Subscriptions**, click **Create subscription**.

![Tên ảnh hiển thị](/images/2.prerequisite/image4.png)

3. Chọn:
   - **Protocol**: `Email`
   - **Endpoint**: nhập địa chỉ email của bạn để nhận thông báo (ví dụ: `user@example.com`)
   ![Tên ảnh hiển thị](/images/2.prerequisite/Untitled.png)

4. Click **Create subscription**.

![Tên ảnh hiển thị](/images/2.prerequisite/image9.png)

> 📧 Một email xác nhận sẽ được gửi tới địa chỉ bạn nhập. **Bạn cần click xác nhận** trong email đó để bắt đầu nhận thông báo.

![Tên ảnh hiển thị](/images/2.prerequisite/image10.png)

---

## 🔐 Tạo IAM User để gửi SNS

Trong phần này, bạn sẽ tạo một **IAM User chuyên dùng để gửi email qua Amazon SNS**. User này sẽ có quyền `sns:Publish` vào topic bạn đã tạo.

---

### 1. Truy cập IAM Console

Truy cập vào [IAM Management Console](https://us-east-1.console.aws.amazon.com/iamv2/home?region=us-east-1#/home).

![IAM Console](/images/2.prerequisite/image5.png)

---

### 2. Tạo IAM User

- Chọn **Users** → nhấn **Create user**
  


---

### 3. Nhập thông tin User

- Nhập tên user, ví dụ: `book-store-sns`
- Tick chọn **Programmatic access**
  
![Create User](/images/2.prerequisite/image6.png)

→ Nhấn **Next**

---

### 4. Gán quyền sử dụng SNS

- Chọn **Attach policies directly**
- Tìm kiếm `AmazonSNSFullAccess` và tick chọn `AmazonSNSFullAccess` (hoặc gán custom policy nếu bạn đã tạo trước đó)

![Set SNS Permissions](/images/2.prerequisite/image7.png)

Nhấn **Next**

### 5. Xác nhận và tạo IAM User

- Nhấn **Create user**
![Set SNS Permissions](/images/2.prerequisite/image8.png)
Tạo Access Key

- Nhấn vào **Create access key** (ở góc phải, mục `Access key`)
- Làm theo các bước hướng dẫn để tạo cặp `Access key ID` và `Secret access key`

> ✅ **Ghi chú**: Bạn chỉ có thể xem `Secret access key` **một lần duy nhất** tại thời điểm tạo. Hãy lưu lại cẩn thận để cấu hình trong ứng dụng.

---

### 6. Sử dụng Access Key trong ứng dụng

Bạn có thể sử dụng Access Key này để gửi email thông qua SNS bằng SDK phù hợp:

- Với **ASP.NET MVC** (AWS SDK for .NET)
- Hoặc **Python**, **Node.js**, **Java**, v.v.


### 7. Cấu hình AWS trong `appsettings.json`

Thêm thông tin cấu hình SNS vào file `appsettings.json` như sau:

```json
{
  "AWS": {
    "AccessKey": "AccessKey của bạn",
    "SecretKey": "SecretKey của bạn",
    "Region": "Region của bạn cấu hình",
    "SnsTopicArn": "SnsTopicArn của bạn"
  }
}

```json

> 🔐 **Cảnh báo bảo mật**: Không nên commit `Access key` và `Secret key` vào GitHub. Thay vào đó, hãy lưu vào biến môi trường hoặc file cấu hình `.env`.
