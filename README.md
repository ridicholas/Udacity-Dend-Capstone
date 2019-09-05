# Udacity-Dend-Capstone
### Nicholas Wolczynski

This repository contains my Capstone project for the Udacity Data Engineering Nanodegree. 

## Project Description

In this project I ETL a massive bitcoin historical transaction dataset into an analytics schema. I transfer the raw source datasets from an S3 bucket I have set up: "s3n://udacity-dend-crypto/bitcoin-historical-data/" into another S3 bucket: "s3n://udacity-dend-crypto/analytics/". The analytics S3 bucket will store the files in parquet format and the tables will be set up based on the analytics schema. This data was prepared for an analytics team that is interested in learning more about historical bitcoin transaction volumes. The data was prepared into an analytics schema that could be easily queried to answer various time based questions like "which market sold a greater volume of bitcoin in a particular month?". I assume the analyst would be using Python and Spark to query the data, which was the reasoning for storing the data as parquet files on S3. 

### Source Data

File 1: Bitstamp minute by minute data in csv format. (~4million rows x 8 Columns)

File 2: Coinbase minute by minute data in json format. (~2million rows x 8 Columns). 

### Analytics Schema

![Schema](https://github.com/ridicholas/Udacity-Dend-Capstone/blob/master/Schema.png)

### Tools Used

My primary goal for this project was to get more exposure in using PySpark and the AWS EMR resource. These are the most useful tools for me as an analyst and data scientist so I focused exclusively on these tools. The entire project is written in a Jupyter Notebook file that should be run from an EMR resource.

## Steps Taken

### Creating the instances
First I created the S3 buckets that were going to store the data. Next, I spun up an AWS EMR Cluster. From there, I opened a Jupyter Notebook in the EMR resource and began coding.

### Loading the data
Loading the data was simple. I used spark to load the source data into Spark dataframes. 

### Exploration and Data Cleaning
I had to figure out how the data needed to be transformed in order for it to fit into the analytics schema. Some changes I had to make: 

- Convert all NAN entries to 0 values. 

- Convert data frame column types to appropriate values (all were strings at first, needed to convert to timestamps, doubles, etc.)

### Transformation

Next I had to create the 3 analytics tables. 

'Time Table' - The time table was created by extracting the timestamp columns from the two source files, combining them, and then taking only the distinct values. From there, I created the additional columns needed for the analytics schema by using appropriate functions on the timestamp column. 

'Markets Table' - This table was relatively simple to make since currently there are just two markets. I created this table directly by converting a list of dictionaries to a PySpark dataframe. 

'Transactions Table' - This table needed to be a complete set of data that combined the two source files. I had to reshuffle and rename the columns and then UNION ALL the tables together. 

### Loading
I loaded the tables into the Analytics Schema folder on S3 as parquet files. 

### Data Quality Checks

I checked whether the rows of the tables had appropriate counts as compared to the source files. 

### Analytics

Finally, I wanted to see if this process was actually useful. I put myself in the shoes of the target audience analytics team and queried from the analytics tables in order to answer the analytics questions posed earlier. 

### Future Data Updates

This was taken from a static data source, but theoretically, this dataset should be updated regularly assuming such data was available. Since we are looking at minute by minute data, if it was available we should be updating this dataset as frequently as we want to be analyzing it. In order to do that, we would need to provide additional code to only append new values from the source rather than recreating the entire dataset each time. 

### Scenario Questions

How you would approach the problem differently under the following scenarios:

If the data was increased by 100x.
  
  If the dataset was increased by 100x we could largely keep the process the same since Spark is a great tool for processing giant datasets. We might want to spin up a more powerful EMR cluster in order to handle a 100x bigger dataset (increase the number of cores and perhaps consider an optimized EMR cluster, depending on if we care more about storage or more about computing power). 

If the pipelines were run on a daily basis by 7am.
  
  I made this in Jupyter Notebook from my point of view as an analyst. However, if we wanted this to be a more production level pipeline that needs to be run at 7am each day, we can convert the Jupyter notebook to a python script and send that directly to the emr instance to run every day at 7am. 


If the database needed to be accessed by 100+ people.
  If the dataset needed to be accessed by 100+ more people there should not really be a problem pulling it from S3. We might want to consider transferring it to the EMR HDFS database to make things a bit quicker. 








