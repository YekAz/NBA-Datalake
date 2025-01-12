# Overview

The setup_nba_data_lake.py script automates the following tasks:

- Provisions an Amazon S3 bucket to hold both raw and processed NBA data.
- Uploads sample NBA data in JSON format to the newly created S3 bucket.
- Establishes an AWS Glue database and defines an external table for data querying.
- Configures Amazon Athena to enable querying of the data stored in the S3 bucket.

# Prerequisites
Before running the script, ensure you have the following:

1. SportsData.io Account and API Key:

    - Visit SportsData.io and create a free account.
    - Hover over "Developers" in the top-left menu and select "API Resources".
    - Click on "Introduction & Testing", then choose "SportsDataIO API Free Trial".
    - Fill out the form, ensuring you select NBA as the focus for this tutorial.
    - After registering, you'll receive an email with a link to the Developer Portal. Click "Launch Developer Portal".
    - By default, you'll see NFL data. On the left sidebar, switch to NBA.
    - Scroll down to the "Standings" section. Under "Query String Parameters", you'll find your API key in the dropdown box.
    - Copy this API key, as you'll need it for the script.
    - IAM Role or Permissions:

2. Ensure the user or role executing the script has the following AWS permissions:
    - S3:
        - s3:CreateBucket
        - s3:PutObject
        - s3:DeleteBucket
        - s3:ListBucket
    - Glue:
        - glue:CreateDatabase
        - glue:CreateTable
        - glue:DeleteDatabase
        - glue:DeleteTable
    - Athena:
        - athena:StartQueryExecution
        - athena:GetQueryResults
    Make sure all prerequisites are in place before running the script.

Step 1: Create the `setup_nba_data_lake.py` File
- Open the File in Your CLI:

    - In the command line interface (CLI), type the following command to create the file:

    `nano setup_nba_data_lake.py`

    - Locate the setup_nba_data_lake.py file and copy its entire contents.

- Paste the Script:

    - Switch back to the CloudShell or CLI window where you opened the file.
    - Paste the copied contents into the editor.

Step 2: Create the .env File
- Open the File in Your CLI:

In the command line interface (CLI), type the following command to create a new file:
```
nano .env
```
Paste the following lines into the file, replacing your_sportsdata_api_key with your actual API key:
```
SPORTS_DATA_API_KEY=your_sportsdata_api_key
NBA_ENDPOINT=https://api.sportsdata.io/v3/nba/scores/json/Players
```
Add the .env file to your .gitignore so as not to expose your API keys to the public (VERY IMPORTANT)
```
echo ".env/" >> .gitignore
```

Step 3: Create a requirements file and install requirements
The project needs requirements such as boto3, requests and pythondotenv
```
nano requirements.txt
```

Copy the requirements below into the file:
```
boto3==1.26.137
requests==2.28.2
python-dotenv==1.0.0
```

Step 4: Create a virtual environment to install the project requirements

Run the following commands:
```
#update package repo
sudo apt update

#install python3-venv package
sudo apt install python3.12-venv

#create the virtual environment
python3 -m venv venv

#Activate the virtual environment
source venv/bin/activate

#install requirements
pip install -r requirements.txt

#add venv to .gitignore
echo "venv/" >> .gitignore
```

Step 5: Run the script
In the CLI type:
```
python3 setup_nba_data_lake.py
```
You should see the resources were successfully created, the sample data was uploaded successfully and the Data Lake Setup Completed

Step 6: Verify the Resources Manually
- Check the S3 Bucket:

    - In the AWS Management Console, use the search bar to locate S3 and open the service.
    - Look for a bucket named Sports-analytics-data-lake (general-purpose bucket).
    - Click on the bucket name to view its contents. You should see three objects inside the bucket.

- Inspect the Data:

    - Navigate to the raw-data folder within the bucket.
    - Confirm that it contains a file named nba_player_data.json.
    - Click on the file name, and at the top of the page, select the option to Open or View the file.
    - The file should display a long string of NBA data in JSON format.

- Query the Data with Athena:

    - Go to Amazon Athena in the AWS Management Console.
    - Paste the following sample query into the query editor:
```
SELECT FirstName, LastName, Position, Team
FROM nba_players
WHERE Position = 'PG';
```

- Click Run to execute the query.
- Scroll down to the Query Results section to view the output. You should see a list of players with the position "PG" (Point Guard).





