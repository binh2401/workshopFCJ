
---
title: "Graph Neural Networks với Neptune ML(chưa xong)"
date: 2025-08-05
weight: 6
chapter: false
pre: "<b> 6. </b>"
---
## 🎯 Mục tiêu
Hướng dẫn xuất dữ liệu từ SQL Server sang file CSV và upload thủ công lên Amazon S3 để sử dụng trong Amazon Neptune.

---

## 🧱 Bước 1: Cài đặt thư viện cần thiết

### Cài đặt bằng `pip`:

```bash
pip install pandas pyodbc boto3
```

---

## 🧩 Bước 2: Kết nối SQL Server và xuất dữ liệu ra thư mục `./export`

### 📁 Cấu trúc thư mục:
```
project/
│
├── export/         # Chứa các file CSV sau khi export
├── data.py         # Script Python để export
```

### 🔢 Code `data.py`:

```python
import os
import pandas as pd
from sqlalchemy import create_engine

# Chuỗi kết nối
server = '(localdb)\\MSSQLLocalDB'
database = 'boookstore'
conn_str = f'mssql+pyodbc://@{server}/{database}?driver=ODBC+Driver+17+for+SQL+Server'

# Tạo engine kết nối
engine = create_engine(conn_str)

# Danh sách bảng cần export
tables = ['dbo.Categories', 'dbo.Products', 'dbo.Orders']

# Thư mục lưu file CSV
output_folder = './export'
os.makedirs(output_folder, exist_ok=True)

# Xuất dữ liệu từng bảng
for table in tables:
    print(f"📌 Đang xử lý bảng: {table}")
    df = pd.read_sql(f"SELECT * FROM {table}", engine)
    table_name = table.split('.')[-1]
    df.to_csv(f'{output_folder}/{table_name}.csv', index=False)
    print(f"✅ Đã lưu: {table_name}.csv")
```

---

## ☁️ Bước 3: Upload thủ công lên Amazon S3

1. Vào [AWS S3 Console](https://s3.console.aws.amazon.com/s3)
2. Tạo một bucket (nếu chưa có)
3. Vào bucket đó → "Upload" → Chọn các file `.csv` trong thư mục `./export`
4. Sau khi upload, copy đường dẫn S3 URI của các file (ví dụ: `s3://your-bucket-name/Categories.csv`)
![FWD](/images/6.clean/s31.png)
![FWD](/images/6.clean/s32.png)
---

