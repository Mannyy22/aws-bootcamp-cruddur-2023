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

I created bash scripting files for this directory, so we could execute task easier:

`db-create`
`db-drob`
`db-schema-load`
`db-seed`
'db-connect`

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

CYAN='\033[1;36m'
NO_COLOR='\033[0m'
LABEL="db-create"
printf "${CYAN}== ${LABEL}${NO_COLOR}\n"

NO_DB_CONNECTION_URL=$(sed 's/\/cruddur//g' <<<"$CONNECTION_URL")
psql $NO_DB_CONNECTION_URL -c "CREATE DATABASE cruddur;"
```

`db-drop`
```
#! /usr/bin/bash

CYAN='\033[1;36m'
NO_COLOR='\033[0m'
LABEL="db-drop"
printf "${CYAN}== ${LABEL}${NO_COLOR}\n"

NO_DB_CONNECTION_URL=$(sed 's/\/cruddur//g' <<<"$CONNECTION_URL")
psql $NO_DB_CONNECTION_URL -c "DROP DATABASE cruddur;"
```

![image](https://user-images.githubusercontent.com/46639580/227412630-24a64f4b-2752-46fb-9edb-8264b045e84c.png)
![image](https://user-images.githubusercontent.com/46639580/227414538-6801b2be-fd70-4127-ba79-0623574f525d.png)


`db-schema-load`

```
#! /usr/bin/bash

CYAN='\033[1;36m'
NO_COLOR='\033[0m'
LABEL="db-schema-load"
printf "${CYAN}== ${LABEL}${NO_COLOR}\n"

echo "db-schema-load"

schema_path="$(realpath .)/db/schema.sql"
echo $schema_path

if [ "$1" = "prod" ]; then
    echo "Running in production mode"
    URL=$PROD_CONNECTION_URL
else
    URL=$CONNECTION_URL
fi

psql $URL cruddur < $schema_path

```

For `db-schema-load`we added an if-statement that allows us to enter a parameter when we run the command, so we can choose to connect to your local database or production database.

![image](https://user-images.githubusercontent.com/46639580/227416282-2528ee60-40bf-47cf-8a3a-0715a0e59cd6.png)


'db-connect`

```
#! /usr/bin/bash

psql $CONNECTION_URL
```

This file allow us to connect to our database prod or local depending on the parameter we give it

`db-seed`

```
#! /usr/bin/bash


CYAN='\033[1;36m'
NO_COLOR='\033[0m'
LABEL="db-seed"
printf "${CYAN}== ${LABEL}${NO_COLOR}\n"

seed_path="$(realpath .)/db/seed.sql"

echo $seed_path

psql $CONNECTION_URL cruddur < $seed_path
```
This file allows us to inset data into our table after it has been created

![image](https://user-images.githubusercontent.com/46639580/227420318-5e3c570f-665c-41e8-ae88-37f741ec84c9.png)

## SQL Files

In addition we directory called `db` and created two `.sql` file in it:
`scheme.sql`
`seed.sql`

`scheme.sql`
This file creates a schema for our data base, schema like an excel file, it setsup how our Database will be formatted.

```
CREATE EXTENSION IF NOT EXISTS "uuid-ossp";

DROP TABLE IF EXISTS public.users;

DROP TABLE IF EXISTS public.activities;

CREATE TABLE public.users (
  uuid UUID DEFAULT uuid_generate_v4() PRIMARY KEY,
  display_name text,
  handle text,
  cognito_user_id text,
  created_at TIMESTAMP default current_timestamp NOT NULL
);

CREATE TABLE public.activities (
  uuid UUID DEFAULT uuid_generate_v4() PRIMARY KEY,
  user_uuid UUID NOT NULL,
  message text NOT NULL,
  replies_count integer DEFAULT 0,
  reposts_count integer DEFAULT 0,
  likes_count integer DEFAULT 0,
  reply_to_activity_uuid integer,
  expires_at TIMESTAMP,
  created_at TIMESTAMP default current_timestamp NOT NULL
);
```

`seed.sql`

This file has data we can use to insert into our database.

```INSERT INTO public.users (display_name, handle, cognito_user_id)
VALUES
  ('Andrew Brown', 'andrewbrown' ,'MOCK'),
  ('Andrew Bayko', 'bayko' ,'MOCK');

INSERT INTO public.activities (user_uuid, message, expires_at)
VALUES
  (
    (SELECT uuid from public.users WHERE users.handle = 'andrewbrown' LIMIT 1),
    'This was imported as seed data!',
    current_timestamp + interval '10 day'
  )
```
