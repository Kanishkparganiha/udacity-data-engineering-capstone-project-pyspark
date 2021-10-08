
## Population Demographics, City Immigration and Tempurature Analysis
### Data Engineering Capstone Project
### Project Summary
This project analysis the US immigration data and its relationhip with the census data along with weather Tempurature and explores the reason most popular cities for immigration, gender distribution of the immigrants, visa-type distribution of the immigrants, average tempurature distribution of various cities.

For the sake of the project we are using the following datasets:

I94 immigration dataset of 2016
City temperature data from Kaggle
US city demographic data from OpenSoft.
PySpark and Pandas are used for Data Extraction, Data Manipulation and Data Exploration. Pyspark was found to be more useful for the import sas type file and large csv files

The project follows the follow steps:

Step 1: Scope the Project and Gather Data
Step 2: Explore and Assess the Data
Step 3: Define the Data Model
Step 4: Run ETL to Model the Data
Step 5: Complete Project Write Up


### Step 1: Scope the Project and Gather Data
Scope
The goal of this project is to extract data from the different sources and design a fact and dimension table for analyzing US immigration data using Tempurature data, US census data and seasonality

### Describe and Gather Data
Describe the data sets you're using. Where did it come from? What type of information is included?

1. I94 Immigration Data: US National Tourism and Trade Office and contains statistics on international visitor arrival in USA and comes from the US National Tourism and Trade Office.
2. World Tempurature Data: Derived from Kaggle and contains average weather tempuratures by city.
3. U.S City Demographic Data: derived from OpenSoft and contains imformation about the demographics of all US cities such as average age, male and female population

### Step 2: Explore and Assess the Data
Explore the Data
Identify data quality issues, like missing values, duplicate data, etc.


### Step 3: Define the Data Model
3.1 Conceptual Data Model¶
The Star schema is chosen as the data model because of its easy of design and effectiveness. Users can write simple to complex queries by joining fact table with the dimension table and perform various OLAP queries

### Staging Tables

#### df_staging_i94
    id
    date
    city_code
    state_code
    age
    gender
    visa_type
    count

#### df_tempurature_staging
    year
    month
    city_code
    city_name
    avg_tempurature
    lat
    log


#### staging_demo_df
    city_code
    state_code
    city_name
    median_age
    pct_male_pop
    pct_female_pop
    pct_veterans
    pct_foreign_born
    pct_native_american
    pct_asian
    pct_black
    pct_hispanic_or_latino
    pct_white
    total_pop


### 3.2 Mapping Out Data Pipelines
Cleaning the dataset on nulls, datatypes and, duplicates
Load the staging tables for the immigrant_df, city_df, monthly_city_temp_df and time_df
Create fact table immigration_df with the information on the immigration count, mapping id in immigrant_df, city_code in city_df and monthly_city_temp_df and date in time_df ensuring referential integrity
Save processed dimension and fact table in parquet for OLAP queries



### 4.1 Data Quality Checks
Explain the data quality checks you'll perform to ensure the pipeline ran as expected. These could include:

Integrity constraints on the relational database (e.g., unique key, data type, etc.)
Unit tests for the scripts to ensure they are doing the right thing
Source/Count checks to ensure completeness
Run Quality Checks

#### Perform quality checks here
def table_exists(df): if df is not None: return True else: return False

if table_exists(immigration_df) & table_exists(city_df) & table_exists(monthly_city_temp_df) & table_exists(time_df) & table_exists(immigration_df): print("data quality check passed") print("dimension tables and fact table exist") print() else: print("data quality check failed") print("table missing...")

def table_not_empty(df): return df.count() != 0

if table_not_empty(df_immigrant) & table_not_empty(city_df) & table_not_empty(monthly_city_temp_df) & table_not_empty(time_df) & table_not_empty(immigration_df): print("data quality check passed!") print("dimension tables and fact table contain records") print() else: print("data quality check failed!") print("null records...")

### 4.2 Data dictionary
Create a data dictionary for your data model. For each field, provide a brief description of what the data is and where it came from. You can include the data dictionary in the notebook or in a separate file.

![schema](star_schema.png)

#### Fact Table

#### immigration_df
    id: id
    state_code: state code of arrival city
    city_code: city port code of arrival city
    date: date of arrival
    count: count of immigrant's entries into the US

#### Dimension Tables
#### immigrant_df
    id: id of immigrant
    gender: gender of immigrant
    age: age of immigrant
    visa_type: immigrant's visa type


####  monthly_city_temp_df
    city_code: city port code
    year: year
    month: month 
    avg_temperature: average temperature in city for given month

#### city_df
    city_code: city port code
    state_code: state code of the city
    city_name: name of the city
    median_age: median age of the city
    pct_male_pop: city's male population in percentage
    pct_female_pop: city's female population in percentage
    pct_veterans: city's veteran population in percentage
    pct_foreign_born: city's foreign born population in percentage
    pct_native_american: city's native american population in percentage
    pct_asian: city's asian population in percentage
    pct_black: city's black population in percentage
    pct_hispanic_or_latino: city's hispanic or latino population in percentage
    pct_white: city's white population in percentage
    total_pop: city's total population
    lat: latitude of the city
    long: longitude of the city

#### time_df
    date: date
    dayofweek: day of the week
    weekofyear: week of year
    month: month
##### Step 5: Complete Project Write Up
Pyspark is an interface for the Apache Spark in Python. It's not only allows you to write spark application using Python API. Pyspark support most of the spark's features such as Spark SQL, Dataframe, Streaming, MLib, and Spark Core. Pypark can read various different data types like csv,sas and parquet.

The data update cycle is typically chosen on two criteria. One is the reporting cycle, the other is the availabilty of new data to be fed into the system. For example, if new batch of average temperature can be made available at monthly interval, we might settle for monthly data refreshing cycle.

#### There are also considerations in terms of scaling existing solution.

1. If the data was increased by 100x: We can select high instances of EC2s hosting Spark and/or additional Spark work nodes. With added capacity arising from either vertical scaling or horizontal scaling, we should be able to accelerate processing time.
2. If the data populates a dashboard that must be updated on a daily basis by 7am every day: We can consider using Airflow to schedule and automate the data pipeline jobs. Built-in retry and monitoring mechanism can enable us to meet user requirement.
3. If the database needed to be accessed by 100+ people: We can consider hosting our solution in production scale data warehouse in the cloud, with larger capacity to serve more users, and workload management to ensure equitable usage of resources across users.
