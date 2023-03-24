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

##  Create Schema for Postgres

I created a new folder SQL file in my backend directory called `db` and I created a file inside the folder called `schema.sql`

Added the following to the `schema.sql` file:

`CREATE EXTENSION IF NOT EXISTS "uuid-ossp";`

Then ran the following command below to add the UUID extention to your cruddur database:
```
psql cruddur < db/schema.sql -h localhost -U postgres
```


## Bash scripting for common database actions

I created new folder in backend directory(backend-flask/bin) called `bin` bin stands for Binaries this is where you can find executable files.

I created three files for this directory:

`db-create`
`db-drob`
`db-schema-load`

These are the files we will write our bash script in. It is important to note when you create new files in Linux they will not be executable, so you must change the permissios. You can do that by using the `chmod` command:

Example: `chmod u+x bin/db-create`
        `chmod u+x bin/db-drop`
        `chmod u+x bin/db-schema-load`
        
Now these files are executable(Before they were not):

![image](https://user-images.githubusercontent.com/46639580/227411486-70d0a461-c3d6-4a65-91db-6123913b0474.png) 

I added the following to my files

`db-create`
```
#! /usr/bin/bash

echo "db-drop"

NO_DB_CONNECTION_URL=$(sed 's/\/cruddur//g' <<<"$CONNECTION_URL")
psql $NO_DB_CONNECTION_URL -c "CREATE DATABASE cruddur;"
```

`db-drop`
```
#! /usr/bin/bash

echo "db-drop"

NO_DB_CONNECTION_URL=$(sed 's/\/cruddur//g' <<<"$CONNECTION_URL")
psql $NO_DB_CONNECTION_URL -c "DROP DATABASE cruddur;"
```
`db-schema-load`
````
#! /usr/bin/bash

echo "db-schema-load"

schema_path="$(realpath .)/db/schema.sql"
echo $schema_path

if [ "$1" = "prod" ]; then
    echo "using production"
    URL=$PROD_CONNECTION_URL
else
    URL=$CONNECTION_URL
fi
```

![image](https://user-images.githubusercontent.com/46639580/227412630-24a64f4b-2752-46fb-9edb-8264b045e84c.png)
![image](https://user-images.githubusercontent.com/46639580/227414538-6801b2be-fd70-4127-ba79-0623574f525d.png)

