# Udacity-Dend-Capstone
### Nicholas Wolczynski

This repository contains my Capstone project for the Udacity Data Engineering Nanodegree. 

## Project Description

In this project I ETL a massive bitcoin historical transaction dataset into an analytics schema. I transfer the raw source datasets from an S3 bucket I have set up: "s3n://udacity-dend-crypto/bitcoin-historical-data/" into another S3 bucket: "s3n://udacity-dend-crypto/analytics/". The analytics S3 bucket will store the files in parquet format and the tables will be set up based on the analytics schema. 

### Source Data

File 1: Bitstamp minute by minute data in csv format. (~4million rows x 8 Columns)
File 2: Coinbase minute by minute data in json format. (~2million rows x 8 Columns). 

### Analytics Schema





