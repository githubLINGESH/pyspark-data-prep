pyspark-modifier
A simple and robust utility package for modifying PySpark DataFrames.

pyspark-modifier encapsulates common PySpark DataFrame operations, such as adding new rows, into a clean and reusable class. This makes your data processing code more modular, readable, and easier to maintain.

Features
Encapsulation: The DataFrameModifier class provides a single, organized interface for common DataFrame operations.

Robustness: The add_rows method uses unionByName(), which handles schema alignment by column name, ensuring consistency and preventing errors.

Non-Destructive: All methods return a new DataFrame, adhering to the immutability principle of PySpark.

Scalable: Built on PySpark's core principles, the package is designed to handle large datasets efficiently.

Installation
You can install this package directly from PyPI using pip.

pip install pyspark-modifier

Usage
The primary component of this package is the DataFrameModifier class. Here is a complete example demonstrating how to use the add_rows method to append new data to an existing DataFrame.

from pyspark.sql import SparkSession
from pyspark.sql.types import StructType, StructField, StringType, IntegerType
from pyspark_modifier.dataframe_modifier import DataFrameModifier

# 1. Initialize Spark Session
spark = SparkSession.builder.appName("DataFrameModifier Example").getOrCreate()

# 2. Define schema and create an initial DataFrame
schema = StructType([
    StructField("name", StringType(), True),
    StructField("age", IntegerType(), True),
])
initial_data = [("Alice", 25), ("Bob", 30)]
initial_df = spark.createDataFrame(data=initial_data, schema=schema)

print("--- Original DataFrame ---")
initial_df.show()
initial_df.printSchema()

# 3. Instantiate the modifier class
modifier = DataFrameModifier(spark)

# 4. Data for the new rows (a list of tuples)
new_rows_data = [("Charlie", 35), ("Dana", 40)]

# 5. Add the new rows using the class method
modified_df = modifier.add_rows(initial_df, new_rows_data)

print("\n--- New DataFrame after adding rows ---")
modified_df.show()
modified_df.printSchema()

# 6. Stop the SparkSession when done
spark.stop()