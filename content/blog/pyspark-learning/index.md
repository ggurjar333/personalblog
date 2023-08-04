---
title: PySpark
date: "2023-08-01T22:12:03.284Z"
description: "Quick guide to PySpark and things to keep in mind."
---

## What are the features of **PySpark**?

PySpark, in the context of data engineering, is a robust tool that supports Spark's core functionalities including Spark SQL, DataFrame manipulation, Streaming, and Machine Learning capabilities.



### Spark SQL and DataFrame
Spark SQL structures the data in the form of DataFrames while processing in PySpark. A DataFrame is a distributed collection of data organized into named columns, which provides a programming abstraction for structured data. This allows Spark to run computations on large scale data in a distributed manner.

### MLib (aka Machine Learning library)
MLlib is a scalable machine learning library in Spark, offering high-level APIs for creating and optimizing machine learning pipelines, ideal for processing large-scale data.

MLlib differs from other machine learning libraries in its scalability and compatibility with Spark's distributed computing model. It's designed to perform machine learning algorithms on large datasets across multiple nodes, without the need for data sampling or other approximations. It also integrates seamlessly with other Spark components like Spark SQL and Datasets, making it easier to build end-to-end data pipelines. Additionally, it provides high-level APIs for common machine learning tasks, making it more user-friendly than some low-level libraries.

### Spark Core
Spark Core is the fundamental, general execution engine for the Spark platform. All other functionalities in Spark are built on top of it. It provides Resilient Distributed Datasets (RDDs) and in-memory computing capabilities. RDD is a fundamental data structure of Spark that is an immutable distributed collection of objects, which can be processed in parallel. In-memory computing capabilities mean that Spark can store data in RAM instead of disk which makes the data processing much faster.

> ## How to memorize these?
>
> 1. Start with Spark Core as the base layer. This is the fundamental execution engine where all other functionalities are built on. It provides RDDs and in-memory computing capabilities.
> 2. The next layer could be Spark SQL and DataFrame. This module processes structured data using DataFrames, which are distributed collections of data organized into named columns.
> 3. The top layer could be MLib. This is the machine learning library in Spark that offers high-level APIs for creating machine learning pipelines.




## How to create a dataframe in PySpark?
Learning PySpark after knowing pandas would be moderately easy as both use DataFrames. However, the key difference is PySpark's distributed computing, which might require understanding new concepts. Practice and patience would be needed to master it.

In PySpark, a DataFrame can be created using the pyspark.sql.SparkSession.createDataFrame method. This method can take various inputs like a list of lists, tuples, dictionaries, pyspark.sql.Rows, a pandas DataFrame, or an RDD consisting of such a list. The 'schema' argument can be used to specify the DataFrame's schema. If it's not provided, PySpark will infer the schema by sampling the data.

Firstly, you can create a PySpark DataFrame from a list of rows

```python
## create a spark session
from pyspark.sql import SparkSession


from datetime import datetime, date
import pandas as pd
from pyspark.sql import Row

spark = SparkSession.builder.getOrCreate()
df = spark.createDataFrame([
    Row(
        customer_name="Gaurav",
        order_amt=150,
        order_date=date(2023, 8, 1), order_delivery_time=datetime(2023, 8, 3, 10, 0)
    ),
    Row(
        customer_name="John",
        order_amt=180,
        order_date=date(2023, 8, 1),
        order_delivery_date=datetime(2023, 8, 4, 8, 0)
    ),
    Row(
        customer_name="Abdul",
        order_amt=150,
        order_date=date(2023, 7, 29),
        order_delivery_date=datetime(2023, 8, 2, 8, 0)
    )
])
```
