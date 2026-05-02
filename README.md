# 📦 Using AWS Elastic Beanstalk to Set Up RDS and Access It from an EC2 Instance

## 📖 Project Overview  
This project demonstrates deploying a Flask web application using AWS Elastic Beanstalk integrated with Amazon RDS (MySQL). It also includes secure database access from a separate EC2 instance, monitoring using CloudWatch, and optional use of Secrets Manager.

---

## 🏗️ Architecture Diagram  

### 🔹 Application Flow
```
User Request
↓
Elastic Beanstalk
↓
EC2 (Beanstalk Instance)
↓
Flask App
↓
Amazon RDS
↓
Data Response
```

### 🔹 Additional Components
```
Manual EC2 (Testing)
↓
Connects to RDS using MySQL / Python script
```

```
CloudWatch
↑
Monitors EC2 + Application logs
```

---

## 🛠️ Technologies Used & Purpose  

| Service | Purpose |
|--------|--------|
| AWS Elastic Beanstalk | Application deployment & management |
| Amazon EC2 (Beanstalk) | Runs Flask app (auto-created by Beanstalk) |
| Amazon EC2 (Manual) | Used for testing and accessing RDS database |
| Amazon RDS (MySQL) | Managed relational database |
| VPC, Subnets, Security Groups | Network isolation & security |
| AWS IAM | Access control |
| Amazon CloudWatch | Monitoring |
| AWS Secrets Manager (Optional) | Secure credential storage |

---

## ⚙️ Step-by-Step Implementation  

### 1. Elastic Beanstalk Environment Setup  
- Created Web Server Environment using Elastic Beanstalk  
- Platform: Python (Flask)  
- Instance Type: t3.micro  
- Enabled RDS during setup  
- Ensured both EC2 and RDS are in the same VPC  

---

### 2. RDS Configuration  
- Engine: MySQL  
- Instance Type: db.t3.micro  
- Database created within same VPC  

**Security Group Configuration:**
- Allowed port 3306 access only from:
  - Elastic Beanstalk EC2 Security Group  
  - Separate EC2 instance  
- Public access: Disabled  

---

### 3. EC2 Instance Setup (Manual)  

```bash
ssh -i key.pem ubuntu@<EC2-PUBLIC-IP>

sudo apt update
sudo apt install mysql-client python3-pip -y

mysql -h <RDS-ENDPOINT> -u admin -p
```

✔ Successfully accessed database from EC2  

---

### 4. Test Script for RDS (Read/Write)  

```python
import pymysql

connection = pymysql.connect(
    host="<RDS-ENDPOINT>",
    user="admin",
    password="<PASSWORD>",
    database="ebdb"
)

cursor = connection.cursor()

# Create table
cursor.execute("""
CREATE TABLE IF NOT EXISTS users (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(50)
)
""")

# Insert data
cursor.execute("INSERT INTO users (name) VALUES ('TestUser')")
connection.commit()
```

**Run:**
```bash
python3 test_db.py
```

✔ Verified:
- Insert operation  
- Read operation  

---

### 5. CloudWatch Monitoring  
Enabled monitoring using Amazon CloudWatch  

**Metrics:**
- CPU Utilization  
- Network In / Out  

**Steps:**
- CloudWatch → Metrics → EC2 → Per-Instance  
- Select instance  
- View real-time graphs  

---

### 6. Secrets Manager (Optional)  

```python
import boto3
import json

def get_secret():
    client = boto3.client("secretsmanager", region_name="us-east-1")
    response = client.get_secret_value(SecretId="myrds-secret")
    return json.loads(response["SecretString"])
```

✔ Avoids hardcoding credentials  

---

## 🔐 Security Considerations  
- RDS is not publicly accessible  
- Access restricted via Security Groups  
- IAM roles used  
- Secrets Manager for credentials  
- No hardcoded passwords  

---

## 📸 Screenshots  
Add the following:  
1. Elastic Beanstalk environment (Running)  
2. RDS instance  
3. EC2 instances (Beanstalk + Manual)  
4. IAM roles / policies  
5. EC2 to RDS connection  
6. Test script output  
7. Flask application output  
8. CloudWatch CPU metrics  
9. CloudWatch Network metrics  

---

## 🔄 Traffic Flow  

```
Client Request
↓
Elastic Beanstalk URL
↓
EC2 Instance (Flask App)
↓
Amazon RDS (MySQL)
↓
Response
```

---

## ⚠️ Challenges & Solutions  

| Issue | Solution |
|------|---------|
| Unable to connect to RDS | Fixed security group rules |
| Deployment issues | Corrected project structure |
| Database connection error | Used correct endpoint |
| Package installation error | Used virtual environment |

---

## ✅ Conclusion  
- Successfully deployed Flask application using Elastic Beanstalk  
- Integrated Amazon RDS securely  
- Validated database connectivity via EC2  
- Implemented monitoring using CloudWatch  
- Followed best practices  

---

## 👩‍💻 Author  

**Pooja Dange**  
Cloud & DevOps Learner  

📧 Email: poojadange1501@gmail.com  
🔗 LinkedIn: https://www.linkedin.com/in/pooja-dange-0270072b3  
💻 GitHub: https://github.com/pooja-dange1501/-Beanstalk-.git
