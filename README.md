# NBADataLake
This repository contains the setup_sport_data_lake.py script, which automates the creation of a serverless data lake for NBA analytics using AWS services. It integrates Amazon S3, AWS Glue, and Amazon Athena to store, process, and query NBA-related data efficiently

# Overview
The setup_sport_data_lake.py script simplifies the setup of a data lake by performing the following tasks:

- Amazon S3:

  - Creates an S3 bucket to store raw and processed NBA data.
  - Uploads sample NBA data in JSON format to the S3 bucket.
- AWS Glue:

  - Creates a Glue database and a schema (external table) for the S3 data.
  - Enables the data to be queryable by cataloging metadata and defining table structures.
- Amazon Athena:

  - Configures Athena to execute SQL queries directly on the Glue Catalog data stored in S3.

# Prerequisites
Before running the script, ensure you have the following:
- NBA API Access: Obtain a free account and API key from [sportsdata.io](https://sportsdata.io/)
- IAM Role/Permissions: Ensure the user or role running the script has the following permissions:
 - S3: s3:CreateBucket, s3:PutObject, s3:DeleteBucket, s3:ListBucket
 - Glue: glue:CreateDatabase, glue:CreateTable, glue:DeleteDatabase, glue:DeleteTable
 - Athena: athena:StartQueryExecution, athena:GetQueryResults

## **Project Structure**
```bash
 SPORT-DATA-LAKE/
├── src/
│   ├── setup_sport_data_lake.py      # Script to set up the data lake
|   ├── .env                          # Environment variables for AWS configuration
├── policies/
│   ├── IAM_Role                      # IAM role definition and permissions file
├── delete_aws_resources              # Script or directory for removing AWS resources
└── README.md                         # Documentation for the project
```

# START HERE 
# Step 1: Open CloudShell Console

1. Go to aws.amazon.com & sign into your account

2. In the top, next to the search bar you will see a square with a >_ inside, click this to open the CloudShell

# Step 2: Create the setup_nba_sport_lake.py file
1. In the CLI (Command Line Interface), type
```bash
nano setup_sport_data_lake.py
```


2. In another window, go to [GitHub](https://github.com/cloudwithoyindamola/NBASportDataLake)

- Copy the contents inside the `setup_sport_data_lake.py`file

- Go back to the Cloudshell window and paste the contents inside the file.

3. Find the line of code under #Sportsdata.io configurations that says "api_key" 
- paste your api key inside the quotations

- Change the bucket name to your unique bucket name

4. Press ^X to exit, press Y to save the file, press enter to confirm the file name 


# Step 3: Create .env file
1. In the CLI (Command Line Interface), type
```bash
nano .env
```
2. paste the following line of code into your file, ensure you swap out with your API key
```bash
SPORTS_DATA_API_KEY=your_sportsdata_api_key
NBA_ENDPOINT=https://api.sportsdata.io/v3/nba/scores/json/Players
```

3. Press ^X to exit, press Y to save the file, press enter to confirm the file name 


# Step 4: Run the script
1. In the CLI type
```bash
python3 setup_sport_data_lake.py
```
-You should see the resources were successfully created, the sample data was uploaded successfully and the Data Lake Setup Completed

# Step 5: Manually Check For The Resources
1. In the Search Bar, type S3 and click blue hyper link name

- You should see 2 General purpose bucket named (or your bucket name)"Sports-analytics-data-lake"

- When you click the bucket name you will see 3 objects are in the bucket

2. Click on raw-data and you will see it contains "nba_player_data.json"

3. Click the file name and at the top you will see the option to Open or Download  the file

-You'll see a long string of various NBA data


4. Head over to Amazon Athena and you could paste the following sample query:
```bash
SELECT FirstName, LastName, Position, Team
FROM nba_players
WHERE Position = 'PG';
```

-Click Run
-You should see an output if you scroll down under "Query Results"

# Optional AWS Resource Deletion Script
To delete some AWS resources and manage cost,Run the `delete_aws_resources` file
This script deletes the following specific AWS resources:

1. S3 Bucket
   - Target: "sports-analytics-data-lake"
   - Deletes all objects within the bucket
   - Removes the entire bucket

2. AWS Glue Resources
   - Target: "glue_nba_data_lake" database
   - Deletes all tables associated with the database
   - Removes the entire Glue database

3. Athena Query Results
   - Removes query results stored in the "athena-results/" prefix of the S3 bucket

Key Benefits:
- Helps control cloud spending
- Quickly removes unused resources
- Prevents unnecessary cost accumulation

Caution: Ensure you want to permanently delete these resources before running the script.
