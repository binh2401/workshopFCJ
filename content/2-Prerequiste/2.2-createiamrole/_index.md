---
title: "Creating SNS and IAM for Sending Emails"
date: "`r Sys.Date()`"
weight: 3
chapter: false
pre: "<b> 2.3 </b>"
---

### 📬 Objective

In this step, we will:

- Create an **SNS Topic** to send emails when an order is successfully placed.
- Create an **IAM Policy** that allows the application to send notifications via SNS.
- Attach this permission to the EC2 instance or backend service.

---

## 🛠️ Create an SNS Topic

1. Go to the [Amazon SNS service](https://console.aws.amazon.com/sns/v3/home).  
   ![Displayed image name](/images/2.prerequisite/image.png)

2. Select **Topics** from the left menu, then click **Create topic**.  
   ![Displayed image name](/images/2.prerequisite/image2.png)

3. Choose **Standard** as the topic type.  
4. Enter a topic name, e.g., `book-email`.  
   ![Displayed image name](/images/2.prerequisite/image3.png)

5. Keep the default settings and click **Create topic**.

---

## 📥 Subscribe an Email to Receive Notifications

1. After creating the topic, select it from the list.
2. In the **Subscriptions** tab, click **Create subscription**.  
   ![Displayed image name](/images/2.prerequisite/image4.png)

3. Select:
   - **Protocol**: `Email`
   - **Endpoint**: enter your email address to receive notifications (e.g., `user@example.com`)  
     ![Displayed image name](/images/2.prerequisite/Untitled.png)

4. Click **Create subscription**.  
   ![Displayed image name](/images/2.prerequisite/image9.png)

> 📧 A confirmation email will be sent to the provided address. **You must click the confirmation link** in that email to start receiving notifications.

![Displayed image name](/images/2.prerequisite/image10.png)

---

## 🔐 Create IAM User to Send SNS Notifications

In this section, you will create an **IAM User dedicated to sending emails via Amazon SNS**. This user will have the `sns:Publish` permission for the topic you created.

---

### 1. Access IAM Console

Visit the [IAM Management Console](https://us-east-1.console.aws.amazon.com/iamv2/home?region=us-east-1#/home).

![IAM Console](/images/2.prerequisite/image5.png)

---

### 2. Create IAM User

- Go to **Users** → click **Create user**

---

### 3. Enter User Details

- Enter a username, e.g., `book-store-sns`
- Tick **Programmatic access**  

![Create User](/images/2.prerequisite/image6.png)

→ Click **Next**

---

### 4. Assign SNS Permissions

- Choose **Attach policies directly**
- Search for `AmazonSNSFullAccess` and tick the box next to it (or attach a custom policy if you already created one)

![Set SNS Permissions](/images/2.prerequisite/image7.png)

Click **Next**

### 5. Review and Create IAM User

- Click **Create user**  
  ![Set SNS Permissions](/images/2.prerequisite/image8.png)

**Create Access Key**:

- Click **Create access key** (top-right in the `Access key` section)
- Follow the instructions to generate the `Access key ID` and `Secret access key`

> ✅ **Note**: You can only view the `Secret access key` **once** during creation. Be sure to store it securely for use in your application.

---

### 6. Use Access Key in Application

You can use this access key to send emails via SNS using an appropriate SDK:

- For **ASP.NET MVC** (AWS SDK for .NET)  
- Or use **Python**, **Node.js**, **Java**, etc.

### 7. Configure AWS in `appsettings.json`

Add the following SNS configuration in your `appsettings.json`:

```json
{
  "AWS": {
    "AccessKey": "YourAccessKey",
    "SecretKey": "YourSecretKey",
    "Region": "YourConfiguredRegion",
    "SnsTopicArn": "YourSnsTopicArn"
  }
}
