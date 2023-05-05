# astro-airflow-dbt-project

## _Instruction_
1. Install `astro` from https://docs.astronomer.io/astro/cli/install-cli?tab=mac#install-the-astro-cli
2. Execute `astro dev start`
3. Create a S3 bucket
4. Put the `order_data_header.csv` file from `include/data`
5. Create `IAM user` with credential type- `Access key - Programmatic access`
6. Attach exisiting neccessary `S3 policies`
7. Download credential 
8. Execute SQL queries below in your Snowflake
   i. CREATE DATABASE ASTRO_SDK_DB;
   ii. CREATE WAREHOUSE ASTRO_SDK_DW;
   iii. CREATE SCHEMA ASTRO_SDK_SCHEMA;
   iv. CREATE OR REPLACE TABLE customers_table (customer_id CHAR(10), customer_name VARCHAR(100), type VARCHAR(10));
   v. INSERT INTO customers_table (CUSTOMER_ID, CUSTOMER_NAME,TYPE) VALUES ('CUST1','NAME1','TYPE1'),('CUST2','NAME2','TYPE1'),('CUST3','NAME3','TYPE2');
   vi. CREATE OR REPLACE TABLE reporting_table (
    CUSTOMER_ID CHAR(30), CUSTOMER_NAME VARCHAR(100), ORDER_ID CHAR(10), PURCHASE_DATE DATE, AMOUNT FLOAT, TYPE CHAR(10));
    vii. INSERT INTO reporting_table (CUSTOMER_ID, CUSTOMER_NAME, ORDER_ID, PURCHASE_DATE, AMOUNT, TYPE) VALUES
    ('INCORRECT_CUSTOMER_ID','INCORRECT_CUSTOMER_NAME','ORDER2','2/2/2022',200,'TYPE1'),
    ('CUST3','NAME3','ORDER3','3/3/2023',300,'TYPE2'),
    ('CUST4','NAME4','ORDER4','4/4/2022',400,'TYPE2');
9. Create connection `aws_default` in airflow
   i. Connection Type: Amazon S3
   ii. Extra: {"aws_access_key_id": {get from credential.csv}, "aws_secret_access_key": {get from credential.csv}}
   iii. Save it
10. Create connection `snowflake_default` in airflow
   i. Connection Type: Snowflake
   ii. Copy account URL from Snowflake and put it in the Host of Connection
   iii. Put schema name, warehouse, database region, role respectively 
   iv.  Save it
11. Open web browser and go to **http://localhost:8080/**
   user: airflow
   password: airflow
12. Switch on the DAG **astro_orders** and click on it.
13. Click the play buttom on the top right for manual triggers (else it will be shceduled at 12am UTC daily)
14. Execute `astro dev stop`