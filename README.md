# **AWS ALB Log Analytics using S3, Athena, and QuickSight**

## **📌 Overview**
This project sets up an end-to-end **AWS ALB (Application Load Balancer) log analytics pipeline** using:
- **S3** (to store ALB logs)
- **Athena** (to query logs)
- **QuickSight** (to visualize the data)

## **🚀 Project Workflow**
1. **Create an ALB** and enable logging to an **S3 bucket**.
2. **Set up an S3 bucket policy** to allow ALB to write logs.
3. **Use AWS Athena** to query and analyze logs.
4. **Connect Athena with QuickSight** for interactive dashboards.
5. **Create visualizations** for traffic patterns, request distribution, and backend performance.

---

## **🔧 Prerequisites**
Ensure you have the following:
- An **AWS account** with necessary permissions
- An **Application Load Balancer** (ALB) deployed
- **S3 bucket** for log storage
- **AWS Athena & AWS Glue** configured
- **Amazon QuickSight** enabled

---

## **🛠️ Steps to Implement**

### **1️⃣ Enable ALB Logging to S3**
- Go to **EC2 > Load Balancers**
- Select your ALB > **Attributes**
- Enable **Access Logs** and specify the **S3 bucket**

### **2️⃣ Set S3 Bucket Policy for ALB Logs**
Update your S3 bucket policy:

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Principal": {
                "AWS": "arn:aws:iam::127311923021:root"
            },
            "Action": "s3:PutObject",
            "Resource": "arn:aws:s3:::athena-ec2-accessing-bucket/*"
        }
    ]
}
```

### **3️⃣ Create an Athena Database and Table**
Create a database:
```sql
CREATE DATABASE alb_logs_db;
```

Create a table for ALB logs:
```sql
CREATE EXTERNAL TABLE alb_logs (
    type string,
    time string,
    elb_name string,
    request_ip string,
    request_port int,
    backend_ip string,
    backend_port int,
    request_processing_time double,
    backend_processing_time double,
    response_processing_time double,
    elb_status_code int,
    backend_status_code int,
    received_bytes bigint,
    sent_bytes bigint,
    request_verb string,
    request_url string,
    request_protocol string,
    user_agent string,
    ssl_cipher string,
    ssl_protocol string
)
ROW FORMAT SERDE 'org.apache.hadoop.hive.serde2.RegexSerDe'
STORED AS TEXTFILE
LOCATION 's3://your-alb-logs-bucket/AWSLogs/'
TBLPROPERTIES ("skip.header.line.count"="2");
```

### **4️⃣ Query ALB Logs Using Athena**
Example queries:

- **Get Most Frequent Client IPs:**
```sql
SELECT request_ip, COUNT(*) AS request_count 
FROM alb_logs 
GROUP BY request_ip 
ORDER BY request_count DESC 
LIMIT 10;
```

- **Find Most Accessed URLs:**
```sql
SELECT request_url, COUNT(*) AS hits 
FROM alb_logs 
GROUP BY request_url 
ORDER BY hits DESC 
LIMIT 10;
```

### **5️⃣ Connect Athena to QuickSight for Visualization**
- Go to **QuickSight > Datasets > New Dataset**
- Choose **Athena** as the data source
- Select `alb_logs_db` and `alb_logs`
- Create dashboards to analyze traffic trends, error rates, and performance

---

## **📊 Example Visualizations in QuickSight**
- **Request distribution by client IP** (Bar Chart)
- **Most accessed URLs** (Pie Chart)
- **Backend processing time analysis** (Line Graph)
- **HTTP status code breakdown** (Stacked Bar Chart)

---

## **🎯 Benefits**
✅ **Scalable** log analysis with AWS-managed services  
✅ **SQL-based queries** on raw logs using Athena  
✅ **Real-time insights** with QuickSight dashboards  
✅ **Automated** monitoring for ALB performance & security  

---

## **📌 Next Steps**
- Automate **Athena queries** using **AWS Lambda**
- Set up **alerts for anomalies** (e.g., 5xx errors)
- Enable **scheduled reports in QuickSight**

