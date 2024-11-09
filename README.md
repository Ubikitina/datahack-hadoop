# Movie Market Analysis with Hadoop

This project aims to establish a Hadoop cluster for a startup company to analyze the current movie market and derive actionable insights. Leveraging data from a public dataset, [MovieLens](https://grouplens.org/datasets/movielens/), the project is part of the Big Data Architecture & Engineering Master at Datahack.

**Technologies used:**
- Hadoop
- Hive
- Sqoop
- MySQL

> **Note:** This project was deployed on a virtual machine running Cloudera.



## Table of contents
  - [1. Hadoop Ingestion, and usage of Hive, Sqoop and MySQL](#1-hadoop-ingestion-and-usage-of-hive-sqoop-and-mysql)
    - [Objective](#objective)
    - [Data ingestion and Exploration](#data-ingestion-and-exploration)
    - [Query efficiency](#query-efficiency)
  - [2. Hadoop Cluster Sizing](#2-hadoop-cluster-sizing)



## 1. Hadoop Ingestion, and usage of Hive, Sqoop and MySQL

### Objective
Deploy a Hadoop cluster and analyze movie data from a public dataset provided in the [sample_dataset-main](sample_dataset-main) folder of this repository.

See implementation in: [Task_1.md](Task_1.md)


### Data ingestion and Exploration
The startup needs market insights for advertising strategy. The following queries will be performed:

1. Which movie has the most reviews?
2. Who are the top 10 most active users in rating movies?
3. What are the top three movies according to scores? And the bottom three?
4. Is there any profession we should target for advertising efforts? Why?
5. Can you think of any other valuable insights we could extract from the processed data? How?

To do so, the dataset will be uploaded to Hadoop and queries will be executed with Hive. 

### Query efficiency
The startup is concerned about the efficiency of the query # 4. They request to view these results through a web interface. Implement a relational database containing the data from query # 4.

The relational database will be implemented in MySQL using Sqoop.

## 2. Hadoop Cluster Sizing

The startup wants to expand the Big Data infrastructure to process movie events from various sources (cinemas, streaming platforms, etc.). We will estimate the size of a Hadoop cluster needed for this task. 

Here are the data volume estimates for the upcoming year:

| Source    | Daily Events | Event Size |
|-----------|--------------|------------|
| Source 1  | 10,000       | 15 KB      |
| Source 2  | 120,000      | 300 Bytes  |
| Source 3  | 150,000      | 100 KB     |
| Source 4  | 170,000      | 800 KB     |
| Source 5  | 2,000        | 1500 KB    |

Each machine can have up to 22 disks of 2 Terabytes for storage.

**We need to:**
- Calculate the number of machines needed to store all data volume for the next year.
- Justify why this capacity is required for a Hadoop cluster.

See implementation in: [Task_2.md](Task_2.md)