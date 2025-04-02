
# Real-Time Ride-Sharing Analytics with Apache Spark

This project demonstrates how to process and analyze real-time ride-sharing data using **Apache Spark Structured Streaming**. The data is ingested, parsed, aggregated, and analyzed in real-time to provide insights into driver performance and fare statistics.

---

## Task 1: Ingest and Parse Real-Time Ride Data

### Code Explanation:
- **Spark Session Creation**: A Spark session is created to interact with Spark.
- **Socket Streaming**: The script listens for data from `localhost:9999`, where real-time ride-sharing data is streamed.
- **Schema Definition**: The incoming JSON data is parsed using a predefined schema that includes fields such as `trip_id`, `driver_id`, `distance_km`, `fare_amount`, and `timestamp`.
- **Data Parsing**: The JSON data is parsed into a structured format using the `from_json` function.
- **Writing to CSV**: The parsed data is written into CSV files in the `output/task1/parsed_data` directory. Checkpointing is used to ensure fault tolerance.

### Step-by-Step Execution:
1. **Start the Data Generator** to simulate ride data:
   ```bash
   python data_generator.py
   ```
2. **Run Task 1** to start ingesting and parsing the data:
   ```bash
   python task1.py
   ```
3. The script will write the parsed data into CSV files in the `output/task1/parsed_data` directory.

### Sample Output:
Example of CSV output in `output/task1/parsed_data`:
```csv
trip_id,driver_id,distance_km,fare_amount,timestamp
12345,A1,5.2,12.5,2025-04-02 01:11:41
12346,B2,3.5,10.0,2025-04-02 01:11:42
12347,A1,6.1,15.0,2025-04-02 01:11:43
```

---

## Task 2: Real-Time Aggregations (Driver-Level)

### Code Explanation:
- **Read Streaming Data**: Data is read from the CSV files generated in Task 1.
- **Aggregation**: For each driver, the total fare and average distance are calculated in real-time using aggregation functions `sum` and `avg`.
- **Watermarking**: A watermark of `1 minute` is applied to handle late-arriving data.
- **Saving Aggregated Data**: The aggregation results are written into CSV files in the `output/task2` directory.

### Step-by-Step Execution:
1. Ensure **Task 1** is running and data is being ingested.
2. **Run Task 2** to perform real-time aggregations:
   ```bash
   python task2.py
   ```
3. The script will continuously aggregate the data and write the results to CSV files in the `output/task2` directory.

### Sample Output:
Example of CSV output in `output/task2`:
```csv
driver_id,total_fare,avg_distance
A1,27.5,5.4
B2,10.0,3.5
```

---

## Task 3: Windowed Time-Based Analytics

### Code Explanation:
- **Windowed Aggregation**: This task performs a **5-minute sliding window aggregation** on the `fare_amount` field, which means that every 5 minutes, the total fare is calculated over the last 5 minutes of data, sliding every 1 minute.
- **Watermarking**: The data is also watermarked with a `2-minute` window to allow for late data.
- **Flattening Window**: The window structure (with `start` and `end` timestamps) is flattened into separate columns for better usability.
- **Saving Results**: The aggregated window results are saved into CSV files in the `output/task3` directory.

### Step-by-Step Execution:
1. **Ensure Task 1** and **Task 2** are running and data is available.
2. **Run Task 3** to perform windowed time-based analytics:
   ```bash
   python task3.py
   ```
3. The script will write the windowed aggregation results to CSV files in the `output/task3` directory.

### Sample Output:
Example of CSV output in `output/task3`:
```csv
window_start,window_end,total_fare
2025-04-02 01:00:00,2025-04-02 01:05:00,35.0
2025-04-02 01:01:00,2025-04-02 01:06:00,45.0
```

---

## Commands and Final Execution Instructions:

### 1. **Start Data Generator** (This will simulate ride-sharing data and stream it to the socket):
```bash
python data_generator.py
```

### 2. **Run Task 1** (Ingest and parse the streaming data):
```bash
python task1.py
```

### 3. **Run Task 2** (Perform real-time aggregations for each driver):
```bash
python task2.py
```

### 4. **Run Task 3** (Perform 5-minute sliding window aggregation on fare amount):
```bash
python task3.py
```

---

### Output:
- **Task 1 Output**: CSV files in the directory `output/task1/parsed_data`.
- **Task 2 Output**: Aggregated data for drivers in the directory `output/task2`.
- **Task 3 Output**: Windowed time-based aggregated results in the directory `output/task3`.

---

### Conclusion:
This project demonstrates how to process real-time streaming data, perform aggregations, and analyze trends using **Apache Spark Structured Streaming**. The tasks showcase how to ingest, aggregate, and analyze ride-sharing data in real-time.

