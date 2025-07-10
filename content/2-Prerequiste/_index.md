---
title: "Preparation Steps"
date: "`r Sys.Date()`"
weight: 2
chapter: false
pre: "<b> 2. </b>"
---

{{% notice info %}}
You need to prepare certain AWS resources and programming technologies in advance to ensure a smooth development and deployment process for the system.
{{% /notice %}}

## 💻 Technologies Used

| Technology / Service                    | Role                                                                 |
|----------------------------------------|----------------------------------------------------------------------|
| **AWS EC2**                            | Server to deploy the web application                                |
| **AWS RDS**                            | Cloud-based database service (SQL Server)                           |
| **AWS S3**                             | Stores book cover images and attachments                            |
| **AWS SNS**                            | Sends email notifications when order status changes                 |
| **AWS IAM**                            | Manages access permissions for AWS resources                        |
| **AWS CloudWatch**                     | Logs system activities, monitors server status and performance      |
| **AWS WAF**                            | Protects the website from external threats                          |
| **Graph Neural Networks + Neptune ML** | Recommends books based on user-book-author-category interaction network |

## 🔧 AWS Resources to Prepare

Before integrating system functionalities, you should prepare the following:

- **1 EC2 instance (Linux or Windows)** in a **public subnet** to deploy the web application.
- **1 RDS instance (SQL Server)** to store data such as products, users, and orders.
- **1 IAM Role** with permissions to access SNS, S3, CloudWatch, and other services.
- **1 SNS Topic** with subscribed email addresses for receiving notifications.
- **1 S3 bucket** to store book cover images.
- **AWS WAF configuration** to protect the website against attacks like SQLi, XSS, and DDoS.
- **AWS CloudWatch configuration** to monitor system logs.
- **Neptune ML account setup** to train the AI-based book recommendation model (using GNN).

## 🔗 Reference Documents

- [Introduction to Amazon EC2](https://000004.awsstudygroup.com/en/)
- [Working with Amazon VPC](https://000003.awsstudygroup.com/en/)
