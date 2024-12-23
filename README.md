# ALB-Log-Analysis-Project

# AWS Athena, S3, EC2, and ALB Log Analysis Project

This project demonstrates how to analyze Application Load Balancer (ALB) access logs using Amazon Athena, S3, EC2, and ALB. It involves configuring infrastructure, collecting logs, querying them, and visualizing the data for insightful analysis.

---

## Project Overview

The project includes:
- **ALB Access Logs Collection:** Store logs in S3 for analysis.
- **Query Logs with Athena:** Use SQL-like queries to analyze log data.
- **Visualization with QuickSight:** Present insights from log data in a graphical format.

---

## Implementation Details

### Step 1: Launch EC2 Instances
- Created two EC2 instances, `Server-A` and `Server-B`, with Apache web servers.
- Configured HTML files to differentiate responses:
  - Server-A: "Hello, This is Priyanka Korde from Server A!"
  - Server-B: "Hello, This is Priyanka Korde from Server B!"

### Step 2: Configure Target Group
- Created a target group for the ALB.
- Registered EC2 instances as targets.
- Configured health checks using `/index.html`.

### Step 3: Create Application Load Balancer (ALB)
- Set up an ALB to distribute traffic between `Server-A` and `Server-B`.
- Verified proper load distribution and high availability.

### Step 4: Enable Access Logs
- Created an S3 bucket and configured it to store ALB logs.
- Set up a bucket policy to grant ALB permissions to write logs.

### Step 5: Query ALB Logs with Amazon Athena
- Configured Athena to access logs stored in S3.
- Created an external table in Athena for the ALB logs using the following schema:

```sql
