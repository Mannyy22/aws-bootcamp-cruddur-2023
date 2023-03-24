# Week 4 — Postgres and RDS

## Todo list
- [x] Create RDS Postgres Instance
- [x] Create Schema for Postgres
- [x] Bash scripting for common database actions
- [x] Install Postgres driver in backend application
- [x] Connect Gitpod to RDS instance
- [x] Create AWS Cognito trigger to insert user into database
- [x] Create new activities with a database insert


## Create RDS Postgres Instance

Created RDS Instance using the following command
```
aws rds create-db-instance \
  --db-instance-identifier cruddur-db-instance \
  --db-instance-class db.t3.micro \
  --engine postgres \
  --engine-version  14.6 \
  --master-username xxxxxxxxxxxxxxx \
  --master-user-password xxxxxxxxxxxxxx \
  --allocated-storage 20 \
  --availability-zone us-east-1a \
  --backup-retention-period 0 \
  --port 5432 \
  --no-multi-az \
  --db-name cruddur \
  --storage-type gp2 \
  --publicly-accessible \
  --storage-encrypted \
  --enable-performance-insights \
  --performance-insights-retention-period 7 \
  --no-deletion-protection
  ```

Output:

![image](https://user-images.githubusercontent.com/46639580/227401631-0fc40aa1-e8b5-414e-8d04-4e071cdf6129.png)

