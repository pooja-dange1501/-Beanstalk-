Project : Using AWS Elastic Beanstalk to Set Up RDS and Access It from an EC2 Instance
) 📖 Project Overview
This project demonstrates deploying a Flask web application using AWS Elastic Beanstalk integrated with Amazon RDS (MySQL). It also includes secure database access from a separate EC2 instance, monitoring using CloudWatch, and optional use of Secrets Manager.

🏗️ Architecture Diagram
Flow:

 
 

Technologies Used & Purpose


Service	Purpose
AWS Elastic Beanstalk	Application deployment & management
Amazon EC2 (Beanstalk)	Runs Flask app (auto-created by Beanstalk)
Amazon EC2 (Manual)	Used for testing and accessing RDS database
Amazon RDS (MySQL)	Managed relational database to store application data
VPC, Subnets, Security Groups	Network isolation and secure communication between resources
AWS IAM	Manages roles and permissions for secure AWS access
Amazon CloudWatch	Monitoring performance metrics and logs
AWS Secrets Manager (Optional)	Secure storage of database credentials

H•O• Step-by-Step Implementation


1.	Elastic Beanstalk Environment Setup
•	Created a Web Server Environment using Elastic Beanstalk
•	Platform: Python (Flask)
 
•	Instance Type: t3.micro
•	Enabled RDS database creation during setup
•	Ensured both EC2 and RDS are in the same VP


2.	RDS Configuration
•	Engine: MySQL
•	Instance Type: db.t3.micro
•	Database created within same VPC
Security Group Configuration:
•	Allowed port 3306 (MySQL) access only from:
o	Elastic Beanstalk EC2 Security Group
o	Separate EC2 instance (manual)
•	Public access:
o	Disabled (for security)


3.	EC2 Instance Setup (Manual)

•	Launched a separate EC2 instance in same VPC
•	Connected via SSH:
ssh -i key.pem ubuntu@<EC2-PUBLIC-IP>
•	Install required tools:
sudo apt update
sudo apt install mysql-client python3-pip -y
•	Connect to RDS:
mysql -h <RDS-ENDPOINT> -u admin -p
Successfully accessed database from EC2


4.	Test Script for RDS (Read/Write)
Created a Python script (test_db.py) to verify database operations:

 
 

Run script:
 
✔ Verified:

•	Insert operation
•	Read operation

5.	CloudWatch Monitoring
Enabled monitoring using Amazon CloudWatch:
Metrics monitored:
•	CPU Utilization
•	Network In / Out
Steps:
•	Navigate to CloudWatch → Metrics → EC2 → Per-Instance
•	Selected instance metrics
•	Viewed real-time graphs

6.	Secrets Manager (Optional)
Stored database credentials securely using AWS Secrets Manager. Example:

 
✔ Avoids hardcoding credentials in code

🔐 Security Considerations
•	RDS is not publicly accessible
•	Database access restricted via Security Groups
•	IAM roles used for secure AWS access
•	Secrets Manager used for storing sensitive credentials
•	No hardcoded passwords in application

 —ˆ• 📸 Screenshots
Include the following screenshots:

1.	Elastic Beanstalk environment (Running)

 
2.	RDS instance created via Beanstalk




3.	EC2 instances (Beanstalk + Manual)


 
4.	IAM roles / policies



5.	EC2 to RDS connection (terminal)

 
6.	Test script execution output





7.	Flask application output

 
8.	CloudWatch metrics (CPU)



9.	CloudWatch metrics (Network)


 
 
⬛»⬛ Traffic Flow


 ⚠️ Challenges & Solutions

Issue	Solution
Unable to connect to RDS	Fixed security group rules
Deployment issues	Corrected project structure
Database connection error	Used correct endpoint
Package installation error	Used virtual environment

●◉✅ Conclusion

•	Successfully deployed a Flask application using Elastic Beanstalk
•	Integrated Amazon RDS securely
•	Validated database connectivity via EC2
•	Implemented monitoring using CloudWatch
•	Followed best practices for security and configuratio
    
               👩‍💻  Author
                 Pooja Dange
                  Cloud & DevOps Learner
               📧 Email: poojadange1501@gmail.com
              🔗 LinkedIn: https://www.linkedin.com/in/pooja-dange-0270072b3
               💻 GitHub: (तुझा GitHub link इथे टाक)

 

