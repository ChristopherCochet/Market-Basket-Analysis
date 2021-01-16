# Market Basket Analysis and Association Rules 

**Project description:** In this project we explore how a market basket analysis can be used to analyze a online grocery dataset from Instacart, customer behaviors and provide simple purchase recommendations using association rules and the a priori algorithm   

<kbd> <img src="images/power bi.gif?raw=true"/> </kbd>

The steps we will follow are:
  - [ ] Describe the dataset used for market basket analysis
  - [ ] Pose relevant questions related to the customer and purchase data 
  - [ ] Process, analyze, model and visualize the data to answer these questions
  - [ ] Provide insights into the use of Market Basket Analysis and why it is appropriate for this dataset
  - [ ] Communicate business insights and findings by clearly connecting the business questions and the data used

---

# 0. Background: What Is Market Basket Analysis and What Can It Be Used For ?

  > Market basket analysis is a set of statistical affinity calculations that help managers better understand and  serve their customers by identifying purchase patterns. In simplest terms, this type of analysis shows what combinations of products most frequently occur together in orders. These relationships can then be used to increase profitability through cross-selling, recommendations, promotions, or even the placement of items on a menu or in a store.

  reference: (https://smartbridge.com/market-basket-analysis-101/)

    The key business use cases and benefits of Market Basket Analysis are to:
    * Identify customers purchases and patterns
    * Improve customer purchasing experience
    * Improve product placement
    * Usell / Cross Sell

  We will use this method to analyze a retail dataset from Kaggle.   

  # 1. The Instacart Online Grocery Shopping Dataset (Kaggle)

  The Instacart Online Grocery Shopping Dataset is an anonymized dataset containing a sample of over 3 million grocery orders from more than 200,000 Instacart users.
  
  The data can be accessed on Kaggle here: (https://www.kaggle.com/c/instacart-market-basket-analysis/data)
  > “The Instacart Online Grocery Shopping Dataset 2017”, Accessed from https://www.instacart.com/datasets/grocery-shopping-2017 on 1/13/2021
  > If you have questions about this dataset, you can reach out to us directly at open.data@instacart.com.


  We will be using the following tables from the dataset to perfom our analysis: 

  - orders:
  ```
    order_id: order identifier
    user_id: customer identifier
    eval_set: which evaluation set this order belongs in (see SET described below)
    order_number: the order sequence number for this user (1 = first, n = nth)
    order_dow: the day of the week the order was placed on
    order_hour_of_day: the hour of the day the order was placed on
    days_since_prior: days since the last order, capped at 30 (with NAs for order_number = 1)
  ```

  - products:
  ```
  product_id: product identifier
  product_name: name of the product
  aisle_id: foreign key
  department_id: foreign key
  ```

  - aisles:
  ```
  aisle_id: aisle identifier
  aisle: the name of the aisle
  ```

  deptartments:
  ```
  department_id: department identifier
  department: the name of the department
  ```

  - order_products:
  ``` order_id: foreign key
  product_id: foreign key
  add_to_cart_order: order in which each product was added to cart
  reordered: 1 if this product has been ordered by this user in the past, 0 otherwise
  where SET is one of the four following evaluation sets (eval_set in orders):
  ```
  ## Tracking our progress
    - [X] Describe the dataset used for market basket analysis
    - [ ] Pose relevant questions related to the customer and purchase data 
    - [ ] Process, analyze, model and visualize the data to answer these questions
    - [ ] Provide insights into the use of Market Basket Analysis and why it is appropriate for this dataset
    - [ ] Communicate business insights and findings by clearly connecting the business questions and the data used

---

# 2. Interesting Questions

  Now that we are a bit more familiar with the Instacart grocery dataset, let's plan to investigate the following questions:
  * Which are the most purpolar items purchased ? Which are the least ?
  * What does the distribution of the products purchased look like ?
  * Do customers purchase items together frequently and which products are most often purchased together ?
  * Can we use this information to recommend other products based on a customer’s cart ?

  ## Tracking our progress
    - [X] Describe the dataset used for market basket analysis
    - [X] Pose relevant questions related to the customer and purchase data 
    - [ ] Process, analyze, model and visualize the data to answer these questions
    - [ ] Provide insights into the use of Market Basket Analysis and why it is appropriate for this dataset
    - [ ] Communicate business insights and findings by clearly connecting the business questions and the data used

---
# 3. Process, analyze, model and visualize the data to answer these questions
  
  In this project and the following jupyter notebook, we largely follow the CRISP-DM process :
  * Business understanding - refer to the background information section above
  * Data preparation & understanding - refer to the following notebook (***) 
      We start by loading the data, inspecting and profiling it and checking for quality issues
      We then perfom some EDA and visualize inspect the Instacart retail data
  * Deliver Insights - refer to the following notebook (***) 
      In the analysis notebook we focus on using the preocessed data to answers the key business questions listed and some customer purchase patterns
  * Modeling & Evaluation / Deployment - refer to the following notebook (***) 
      In the modeling notebook we finalize our analysis by modelling the data and using Association Rules to build a simple recommendation engine based on the customer p[urchase patterns

  ## Tracking our progress
    - [X] Describe the dataset used for market basket analysis
    - [X] Pose relevant questions related to the customer and purchase data 
    - [X] Process, analyze, model and visualize the data to answer these questions
    - [ ] Provide insights into the use of Market Basket Analysis and why it is appropriate for this dataset
    - [ ] Communicate business insights and findings by clearly connecting the business questions and the data used


  # 4. Provide insights into the use of Market Basket Analysis and Aswer Questions
  
  1. Which are the most purpolar items purchased ? Which are the least ?
  2. What does the distribution of the products purchased look like ?
  3. Do customers purchase items together frequently and which products are most often purchased together ?
  4. Can we use this information to recommend other products based on a customer’s cart ?

  ## Tracking our progress
    - [X] Describe the dataset used for market basket analysis
    - [X] Pose relevant questions related to the customer and purchase data 
    - [X] Process, analyze, model and visualize the data to answer these questions
    - [X] Provide insights into the use of Market Basket Analysis and why it is appropriate for this dataset
    - [ ] Communicate business insights and findings by clearly connecting the business questions and the data used

## Pre-requisite
  * PSQL CLI installed (http://postgresguide.com/utilities/psql.html)
  * NorthWind database SQL script downloaded to your local environment
  (https://github.com/pthom/northwind_psql/blob/master/northwind.sql)

## PSQL database in a nutshell
  PSQL is the interactive CLI to interact with Postgres databases. 
  PSQL lets user create, query, update and delete POSTGRES tables and databases through the command line. 

## Create the NorthWind database using PSQL
1. Deploy the NorthWind database sql script

   - Create the NorthWind database
   ```
   psql -h dsdj-postgres-db.clpvihbunw2c.ap-southeast-2.rds.amazonaws.com -U postgres -p 5432 -c "CREATE DATABASE \"NorthWind\";"
   ```

      <kbd> <img src="images/PSQL 1.PNG?raw=true"/> </kbd>

   - Create the NorthWind tables
   ```
   psql -h dsdj-postgres-db.clpvihbunw2c.ap-southeast-2.rds.amazonaws.com -U postgres -p 5432 -d NorthWind < northwind.sql
   ```

      <kbd> <img src="images/PSQL 2.PNG?raw=true"/> </kbd>

2. Check that the NorthWind database was properly created with PGADMIN or PSQL

   - PGADMIN 

      <kbd> <img src="images/pgAdmin 4.PNG?raw=true"/> </kbd>

   - PSQL, Once connected to the Northwind Database use: ``` \d ```

      <kbd> <img src="images/PSQL 3.PNG?raw=true"/> </kbd>


### Tracking our progress
  - [X] Deploy an AWS RDS POSTGRES INSTANCE using TERRAFORM (Free Tier)
  - [X] Check connection to the POSTGRES INSTANCE using PGADMIN 
  - [X] Load a relational database (NorthWind) to the RDS POSTGRES instance using PSQL
  - [ ] Connect Power BI Desktop to the RDS POSTGRES NorthWind database
  - [ ] Build a Simple Power BI Dashboard using the NorthWind dabase
  - [ ] Deploy the Power BI dashboard

---

# 4. Connect Power BI Desktop to the RDS POSTGRES NorthWind database
  Now that we have database instance running with the NorthWind tables loaded, we turn our attention 
  to connecting to the tables with Power BI

## Pre-requisite
  * Power BI desktop installed (https://powerbi.microsoft.com/en-us/downloads/)

### Power BI in a nutshell
  Power BI is an ETL and visualization Microsoft tool for business analytics and reporting. 
  It provides interactive visualizations and business intelligence capabilitieto create reports 
  and dashboards which can be deployed in Microsoft and Azure's environment. 

  1. Launch Power BI Desktop
  
  <kbd> <img src="images/power bi 1.PNG?raw=true"/> </kbd>

  2. Connect to the POSTGRES NorthWind database using AWS host connection information

   - Use get data and find the Postgres data source in the list 
    
      <kbd> <img src="images/power bi 2.PNG?raw=true"/> </kbd>
    
   - Enter the Postgres RDS host information and database name
    
      <kbd> <img src="images/power bi 3.PNG?raw=true"/> </kbd>
    
   - Enter the Postgres RDS host information and database name
    
      <kbd> <img src="images/power bi 4.PNG?raw=true"/> </kbd>

  3. Load to the POSTGRES NorthWind tables 

   - Power BI will show the available NorthWind database tables available
      
      <kbd> <img src="images/power bi 5.PNG?raw=true"/> </kbd>

   - Select and load the NorthWind database tables 
      
      <kbd> <img src="images/power bi 6.PNG?raw=true"/> </kbd>

### Tracking our progress
 - [X] Deploy an AWS RDS POSTGRES INSTANCE using TERRAFORM (Free Tier)
 - [X] Check connection to the POSTGRES INSTANCE using PGADMIN 
 - [X] Load a relational database (NorthWind) to the RDS POSTGRES instance using PSQL
 - [X] Connect Power BI Desktop to the RDS POSTGRES NorthWind database
 - [ ] Build a Simple Power BI Dashboard using the NorthWind dabase
 - [ ] Deploy the Power BI dashboard


# 5. Build a Simple Power BI Dashboard using the NorthWind dabase
  We've connected to the NrthWind database with Power BI and now are going to build a simple set of graphs for our dashboard.

  The NorthWind database includes 14 tables and the their relationships - below are a few key tables:
  * Suppliers: Suppliers and vendors of Northwind
  * Customers: Customers who buy products from Northwind
  * Employees: Employee details of Northwind traders
  * Products: Product information
  * Shippers: The details of the shippers who ship the products from the traders to the end-customers
  * Orders and Order_Details: Sales Order transactions taking place between the customers & the company

## Pre-requisite
  * Northwind database and table relationships (https://docs.yugabyte.com/latest/sample-data/northwind)
  * NorthWind's entity relationship diagram is shown below:
  
  <kbd> <img src="images/northwind-er-diagram.png?raw=true"/> </kbd>


## Power BI Modeling and Dashboard Creation
  Once the database table are imported in Power BI, the tool with automatically identify the table relationships 

  1. Check the table relationships in Power BI
  
  <kbd> <img src="images/power bi 7.PNG?raw=true"/> </kbd>

  2. Build charts and graphs for the dashboard (examples below)

   - NorthWind products overview  
    
      <kbd> <img src="images/power bi 8.PNG?raw=true"/> </kbd>
    
   - NorthWind products revenue overview
    
      <kbd> <img src="images/power bi 9.PNG?raw=true"/> </kbd>
    
   - NorthWind customers overview
    
      <kbd> <img src="images/power bi 10.PNG?raw=true"/> </kbd>

### Tracking our progress
 - [X] Deploy an AWS RDS POSTGRES INSTANCE using TERRAFORM (Free Tier)
 - [X] Check connection to the POSTGRES INSTANCE using PGADMIN 
 - [X] Load a relational database (NorthWind) to the RDS POSTGRES instance using PSQL
 - [X] Connect Power BI Desktop to the RDS POSTGRES NorthWind database
 - [X] Build a Simple Power BI Dashboard using the NorthWind dabase
 - [ ] Deploy the Power BI dashboard


## Deploy the Power BI dashboard
  We are done, we now simply need to publish the dashboard to a Power BI Workspace and start analyzing !
  
  <kbd> <img src="images/power bi 11.PNG?raw=true"/> </kbd>

## Tracking our progress
 - [X] Deploy an AWS RDS POSTGRES INSTANCE using TERRAFORM (Free Tier)
 - [X] Check connection to the POSTGRES INSTANCE using PGADMIN 
 - [X] Load a relational database (NorthWind) to the RDS POSTGRES instance using PSQL
 - [X] Connect Power BI Desktop to the RDS POSTGRES NorthWind database
 - [X] Build a Simple Power BI Dashboard using the NorthWind dabase
 - [X] Deploy the Power BI dashboard