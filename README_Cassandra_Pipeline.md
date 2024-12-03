
# README for Cassandra Data Processing Pipeline

---

## **Project Overview**
This project demonstrates a three-layer data pipeline implemented using Apache Cassandra. The pipeline consists of **Bronze**, **Silver**, and **Gold** layers for storing, cleaning, and aggregating sales data, respectively. It utilizes Python and the Cassandra database to process, store, and analyze sales data from a CSV file.

---

## **Project Architecture**
1. **Bronze Layer (Raw Data)**:
   - Stores the raw sales data from the CSV file.
   - Table: `sales_bronze`
   - Purpose: Acts as a raw data store without modifications.

2. **Silver Layer (Cleaned and Filtered Data)**:
   - Stores selected fields for further analysis (e.g., `region`, `country`, `item_type`, `total_profit`).
   - Table: `sales_silver`
   - Purpose: Provides a cleaned and structured version of the data for downstream processes.

3. **Gold Layer (Aggregated Data)**:
   - Consists of three tables with aggregated data:
     - `sales_by_region`: Total profit grouped by region.
     - `sales_by_item_type`: Total units sold grouped by item type.
     - `monthly_sales_summary`: Total revenue grouped by order date.
   - Purpose: Optimized for reporting and business insights.

---

## **Prerequisites**
1. **Python Packages**:
   - `cassandra-driver`
   - `pandas`
   - `uuid`
   - `json`

   Install the required libraries using:
   ```bash
   pip install cassandra-driver pandas
   ```

2. **Cassandra Database**:
   - A secure connect bundle for your Cassandra instance.
   - Keyspace named `cass_keyspace`.

3. **Input File**:
   - CSV file containing sales data (`sales_100.csv`).

---

## **Setup Instructions**

### 1. **Configure the Cassandra Cluster**
- Place your secure connect bundle (e.g., `secure-connect-big-data-cassandra.zip`) in the project directory.
- Ensure that the credentials JSON (`grandh23@students.rowan.edu-token.json`) is in the project folder.
- Update the file paths for:
  - Secure connect bundle: `'secure-connect-big-data-cassandra.zip'`
  - Credentials JSON: `'grandh23@students.rowan.edu-token.json'`
  - CSV file: `'/content/sales_100.csv'`

### 2. **Keyspace and Tables**
The script automatically connects to the Cassandra keyspace (`cass_keyspace`) and creates or replaces the following tables:
- **Bronze Table**: `sales_bronze`
- **Silver Table**: `sales_silver`
- **Gold Tables**: `sales_by_region`, `sales_by_item_type`, `monthly_sales_summary`

### 3. **Run the Script**
Run the Python script to:
1. Connect to Cassandra.
2. Load the raw data into the Bronze table.
3. Process and load the cleaned data into the Silver table.
4. Aggregate and load the results into the Gold tables.

---

## **Pipeline Execution**

### 1. **Bronze Layer**
- Creates a table `sales_bronze` to store the raw data.
- Inserts all rows from the input CSV file.

### 2. **Silver Layer**
- Creates a table `sales_silver` with cleaned data.
- Selects only essential columns (`region`, `country`, `item_type`, `total_profit`) from the Bronze table.

### 3. **Gold Layer**
- Creates and populates three tables:
  - **`sales_by_region`**: Aggregates total profit for each region.
  - **`sales_by_item_type`**: Aggregates total units sold for each item type.
  - **`monthly_sales_summary`**: Aggregates total revenue for each order date.

### 4. **Query Gold Tables**
- Prints the aggregated data from the Gold tables.

---

## **Query Examples**

- Query aggregated sales by region:
  ```sql
  SELECT * FROM sales_by_region;
  ```

- Query aggregated units sold by item type:
  ```sql
  SELECT * FROM sales_by_item_type;
  ```

- Query monthly revenue summary:
  ```sql
  SELECT * FROM monthly_sales_summary;
  ```

---

## **File Descriptions**

| File Name                              | Description                                           |
|----------------------------------------|-------------------------------------------------------|
| `secure-connect-big-data-cassandra.zip`| Secure connect bundle for connecting to Cassandra.    |
| `grandh23@students.rowan.edu-token.json`| Credentials JSON for Cassandra authentication.        |
| `sales_100.csv`                        | Input sales data file.                                |
| `pipeline.py`                          | Python script implementing the data pipeline.         |

---

## **Output Tables**

### 1. **Bronze Table (`sales_bronze`)**
| **Column Name**   | **Data Type** |
|--------------------|---------------|
| `region`          | TEXT          |
| `country`         | TEXT          |
| `item_type`       | TEXT          |
| `sales_channel`   | TEXT          |
| `order_priority`  | TEXT          |
| `order_date`      | TEXT          |
| `order_id`        | UUID          |
| `ship_date`       | TEXT          |
| `units_sold`      | INT           |
| `unit_price`      | FLOAT         |
| `unit_cost`       | FLOAT         |
| `total_revenue`   | FLOAT         |
| `total_cost`      | FLOAT         |
| `total_profit`    | FLOAT         |

### 2. **Silver Table (`sales_silver`)**
| **Column Name**   | **Data Type** |
|--------------------|---------------|
| `order_id`        | UUID          |
| `region`          | TEXT          |
| `country`         | TEXT          |
| `item_type`       | TEXT          |
| `total_profit`    | FLOAT         |

### 3. **Gold Tables**
- **`sales_by_region`**:
  | **Column Name** | **Data Type** |
  |-----------------|---------------|
  | `region`        | TEXT          |
  | `total_profit`  | FLOAT         |

- **`sales_by_item_type`**:
  | **Column Name**   | **Data Type** |
  |--------------------|---------------|
  | `item_type`       | TEXT          |
  | `total_units_sold`| INT           |

- **`monthly_sales_summary`**:
  | **Column Name**  | **Data Type** |
  |------------------|---------------|
  | `order_date`     | TEXT          |
  | `total_revenue`  | FLOAT         |

---

## **Contact Information**
For questions or support, contact: **grandh23@students.rowan.edu**

---
