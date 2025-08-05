---
title : "Create VPC and Configure Settings"
date :  "`r Sys.Date()`" 
weight : 1 
chapter : false
pre : " <b> 2.1.1 </b> "
---

#### Create VPC 
1. Access the [VPC service management console](https://console.aws.amazon.com/vpc/home)
  + Click **Your VPC**.
  + Click **Create VPC**.

![VPC](/images/2.prerequisite/001-createvpc.png)

2. On the **Create VPC** page:
  + In the **Name tag** field, enter **Lab VPC**.
  + In the **IPv4 CIDR** field, enter: **10.10.0.0/16**.

  ![VPC](/images/2.prerequisite/vpc1.png)

  + Click **Create VPC**.

![VPC](/images/2.prerequisite/vpc2.png)
  
  ## ℹ️ Information

- **By default**, *non-default* subnets have the `Auto-assign public IPv4` attribute set to **false**.
- *Default* subnets have this attribute set to **true**.
- *Non-default* subnets created via EC2 Launch Wizard are set to **true** by default.

---

## 🛠️ Steps to Change Configuration

### 1. Access Amazon VPC Console

🔗 [Open Amazon VPC Console](https://console.aws.amazon.com/vpc/)

### 2. Select the subnet to configure

- In the left navigation pane → **Subnets**.
![VPC](/images/2.prerequisite/subnet1.png)
- Select the **subnet** you want to configure.

### 3. Edit the subnet

- Click **Actions → Edit subnet settings**.
![VPC](/images/2.prerequisite/subnet2.png)
- Toggle the `Enable auto-assign public IPv4 address` option as needed.
![VPC](/images/2.prerequisite/subnet3.png)
- Click **Save** to apply changes.
![VPC](/images/2.prerequisite/subnet4.png)

---

> ⚠️ **Note:** This change **only applies to new EC2 instances** created in the subnet after configuration. Running instances will **not be affected**.

---

## ✅ Recommendations

- Make sure your subnet is in a VPC with an Internet Gateway if you want public internet access.
- Double-check your security group and route table to ensure connectivity.
