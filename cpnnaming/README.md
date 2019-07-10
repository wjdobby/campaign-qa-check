
# cpnnaming
Brado Internal Tool to make sure our paid media campaigns follow our campaign naming template.
The package will perform quality check and return campaign names that are not following naming template. 

 ![enter image description here](https://brado.net/wp-content/uploads/2019/03/logo-brado-solo-200w.png)

# Steps:
- Connect to the PostgreSQL database using given credentials and get a list of specific campaign names using **schema** and **table** names

- Use gs2df package to get reference file uploaded on Google Sheets (***Campaign Naming Rules***) and run **campaignnaming-qa script** to check campaign names against rules 

- The ***Campaign Naming Rules*** is a new Google Sheet that ***ONLY*** documents rules for campaign naming template. The Google Sheets synced with the ***Campaign Naming Convention***. So once rules are changed, mannually adjustment is not required. 

- Upload failed campaign naming dataset to a new table (***Failed Campaign Name***) into database 

## Rules for Campaign Naming QA
1. Make sure there are 10 underscores

2. In sections that are not up to the creator (Product & Country), make sure they have the correct information. Ex. Channel needs to contain one of the following: SE, DN, PS, EM, YT, IR, APP.

3. These are "Non-Paid Social" Campaigns. They should not contains "PS" or any Social Tactic/Engine.

5. **New rules should always be updated into the Google Sheets file "Campaign Naming Convention" before check failed campaign names!**


# Installation

Install the whole package 
```sh
$ pip install cpnnaming
```
## Functions in package
* [*retrieve_data*] - connect to the dataset and download qa check campaign names as a data table 
```sh
from cpnnaming.retrieve_data import cntdb
```
* [*qa_check*] - QA check for each campaign names retreived and return campaign names that fails the QA check 
```sh
 from cpnnaming.qa_check import qa_checking
```
* [*upload_result*] - upload campaign names that fails the QA check back to the dataset
 ```sh
from cpnnaming.qa_check import upld_file
```


## Usage

- Enter the schema and table name to download campaign names as a dataframe using  ```retrieve_data.get_data(schema,table) ```
> Note: The template for datatable naming TBD.

- Follow this [instruction](https://pygsheets.readthedocs.io/en/latest/authorization.html) to get a .json file and put it in the same directory with .py file

- Any changes in QA rules need to be updated in ***Campaign Naming Convention*** Google Sheet.

- Import reference excel from Google Sheets API using 
```rules=gs2df.gs2df('client_secret_540551931166-21q6c10ctr74s4t9d83j1muknjvuoo69.apps.googleusercontent.com.json','Campaign Naming Rules')``` and reach different sheets using index ```a[0],a[1],...```
 > Note: **first parameter**: your own .json file, **second parameter**: Google Sheets Name 


- Create a new table called **Campaign Naming Failed** and upload this dataset to a new table using ```upload_table(schema, table, failed_df)```
> Note: Uploaded table will replace existing table every time.
