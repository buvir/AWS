# Amazon Redshift Notes

This document is a structured transcript and hands-on notes for learning **Amazon Redshift**.  
It covers concepts of **big data, data warehousing, OLTP vs OLAP, Redshift setup, querying, ETL, and optimization**.  

Screenshots will be added alongside relevant queries for illustration.  

---

## 📌 Introduction

Amazon Redshift is a fully managed, cloud-based **data warehouse** that allows you to:  
- Store, query, and manage large amounts of data  
- Run **fast analytical queries** at scale  
- Use **SQL-based queries** for easy adoption  
- Scale with **massively parallel processing (MPP)**  

---

📂 Folder Structure for GitHub
Amazon-Redshift-Notes/
│── Amazon_Redshift.md
│── README.md   (optional overview)
│── Amazon_Redshift_screenshots/
      ├── redshift_iam_user_creation.png
      ├── redshift_iam_user_permissions.png
      ├── redshift_cluster.png
      ├── redshift_snapshot.png


## 📌 When You Need Big Data

Questions to ask before choosing Redshift:  
- What type of data will you store?  
- How will data be ingested?  
- How much data will grow daily?  
- What reports or analytics do you need?  

Recommendation: Start small and grow into a **big data solution** as requirements evolve.  

---

## 📌 Database Locks

Relational databases use **locks** to ensure queries return consistent results.  
- Example: Counting orders with blue shirts while new orders are inserted → locks are needed.  
- Issue: As data grows, locks cause delays (red lights analogy).  

Solution: Use **row-level locks** and eventually move to OLTP + OLAP separation.  

---

## 📌 Separating Old and New Data

- **OLTP (Online Transaction Processing)** → Handles fast inserts/updates of new data  
- **OLAP (Online Analytical Processing)** → Handles heavy analytical queries  

Best practice: Keep **recent data in OLTP** and **archive older data in OLAP (Redshift)**.  

---

## 📌 Data Warehouse Types

- **OLAP databases** like **Amazon Redshift** are optimized for analytics  
- Data is sorted/stored based on frequent query patterns (e.g., by **date**)  
- Can combine structured (orders) + unstructured (logs) data via **data lake ingestion**  

---

## 📌 What is Redshift?

- AWS-managed **Postgres-based MPP data warehouse**  
- Supports **federated queries** to join OLTP + OLAP in one SQL query  
- Scales by splitting data across multiple **nodes**  
- Free trial: **750 hours for 2 months** (use cautiously to avoid high billing)  

---

## 📌 Creating a Cluster

Steps:  
1. Login to **AWS Console → Redshift**  
2. Click **Create Cluster**  
3. Choose **Free Trial option** (dc2.large, single node)  
4. Set **Admin username/password**  
5. Wait until cluster status = `Available`  

## 📌 IAM User Creation

When setting up Redshift, create an IAM user with appropriate permissions.

![IAM User Creation](/redshift_iam_user_creation.png)

---

## 📌 IAM Permissions

Attach the following policies:  
- `AmazonRedshiftAllCommandsFullAccess`  
- `AmazonS3ReadOnlyAccess`  

![IAM User Permissions](/redshift_iam_user_permissions.png)

---

## 📌 Creating a Redshift Cluster

Once the IAM role is ready, create the Redshift cluster.

![Redshift Cluster](/redshift_cluster.png)

---

## 📌 Snapshots & Backups

Redshift supports automated & manual snapshots for backup and recovery.

![Redshift Snapshot](/redshift_snapshot.png)



---

## 📌 Querying the Redshift Cluster

1. Navigate to **Cluster Overview → Query Data → Query Editor**  
2. Connect using:  
   - Database: `dev`  
   - User: `awsuser`  

3. Example queries:  

```bash
-- Select first 100 rows from sales
SELECT TOP 100 * 
FROM sales;
```

```bash
-- Count total sales records
SELECT COUNT(salesid) 
FROM sales;
```

➡️ Expected: ~170,000 records in the sample dataset.  



---

## 📌 Getting Your Data into Redshift

1. **Create Tables**  

```bash
CREATE TABLE orders (
   order_id INT,
   customer_id INT,
   order_date DATE,
   amount DECIMAL(10,2)
);
```

2. **Insert Data**  

```bash
INSERT INTO orders 
VALUES (1, 101, '2025-09-18', 250.50);
```

3. **ETL with COPY Command from S3**  

```bash
COPY orders
FROM 's3://your-bucket/orders.csv'
IAM_ROLE 'arn:aws:iam::123456789012:role/RedshiftRole'
DELIMITER ','
IGNOREHEADER 1;
```



---

## 📌 Data Optimization in Redshift

- Redshift distributes data across nodes  
- Use **distribution keys** & **sort keys** wisely  
- Example: Distribute by **order_id** instead of just **year**, to avoid uneven workload  

---

## 📌 Best Practices

- Keep **OLTP small and optimized**  
- Move historical data into **Redshift**  
- Use **COPY command with S3** for bulk load  
- Always **stop & delete unused clusters** to avoid high cost  

---

## 📌 References

- [AWS Redshift Documentation](https://docs.aws.amazon.com/redshift/)  
- Book: *Designing Data-Intensive Applications* (recommended by mentor)  

---
