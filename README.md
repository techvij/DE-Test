# Data Engineer Coding Test

This is a coding test for Data Engineer role.

## Source Data to process

```bash
1. order_detail.csv
```

| Name                    | Type     | Note  | 
| ----------------        |:--------| :-----|
| order_created_timestamp |timestamp   |  format YYYY-MM-DD HH:MM:SS    |  
| status                  | string   |       |
| price                   | integer  |   |
| discount                | float    |   |
| id                      | string   |   |
| driver_id	          | string   |   |
| user_id	          | string   |   |
| restaurant_id           | string   |   |

```bash
2. restaurant_detail.csv
```
 
| Name                  | Type     | Note  | 
| ----------------      |:--------:| :-----|
| id                    | string   |       |  
| restaurant_name       | string   |       |
| category              | string   |   |
| esimated_cooking_time | float    |   |
| latitude              | float    |   |
| longitude	        | float    |   |


## Business requirements
* Create two tables in postgre database with the above given column types.
  * order_detail table using  __order_detail.csv__
  * restaurant_detail table using __restaurant_detail.csv__

* Once we have these two tables in postgre DB, ETL the same tables to Hive with the same names and corresponding Hive data type using the below guidelines
  * Both the tables should be __external table__. 
  * Both the tables should have __parquet file format__. 
  * restaurant_detail table should be partitioned by a column name __dt__ (type string) with a static value __latest__.
  * order_detail table should be partitioned by  column named __dt__ (type string) extracted from __order_created_timestamp__ in the format __YYYYMMDD__.

``` python
Example of dt column

order_created_timestamp: "2019-06-08 17:31:57"
dt: "20190608"

```
* After creating the above tables in Hive, create two new tables __order_detail_new__ and __restaurant_detail_new__ with their respective columns and partitions and add one new column for each table as explained below.
 

| Table Name            | New Column Name     | Logic | 
| ----------------      |:--------| :-----|
| order_detail          | discount_no_null   | replace all the NULL values of __discount__ column with 0    |  
| restaurant_detail      |  cooking_bin  | using __esimated_cooking_time__ column and the below logic     |

| esimated_cooking_time | cooking_bin     |  
| ----------------       |:--------| 
| 10-40                     | 1   |         
| 41-80        | 2   |       
| 81-120               | 3   |   
| greater than 120  | 4    |   

``` python
Final column count of each table (including partition column):
1. order_detail = 9
2. restaurant_detail = 7
3. order_detail_new = 10
4. restaurant_detail_new = 8  
```
## SQL requirements
1. Get the average __discount__ for each __category__
2. Row count per each __cooking_bin__

## CSV output requirements
Save the above query output to CSV files name __discount.csv__ and __cooking.csv__.

## Technical Requirements
* Use any of the __big data__ / other frameworks (Use Dockers if needed).
* Include a README file that explains how we can deploy your code.
