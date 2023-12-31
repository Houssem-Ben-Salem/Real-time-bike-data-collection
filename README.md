# Real-time-bike-data-collection 

This comprehensive guide walks you through setting up a robust, real-time bike-info data pipeline using Kafka, Apache Spark, Elasticsearch, and Kibana. This end-to-end solution empowers you to collect, analyze, visualize, and store bicycle station data, offering dynamic insights into station availability and trends.

## Overview

Our project harnesses Kafka for the real-time ingestion of bicycle station data, processes and analyzes this data using Spark, stores the results in Elasticsearch for quick retrieval, and visualizes the findings through a Kibana dashboard. This pipeline not only offers a snapshot of current availability but also stores historical data in Hive, a Hadoop DataLake, for deep, retrospective analysis.

### System Architecture

The following diagram illustrates our project pipeline:

![Pipeline Overview](https://github.com/Houssem-Ben-Salem/Real-time-bike-data-collection/assets/93081419/a5427cd0-9f6b-4ebe-a9fe-b64898b97f35)

## Prerequisites

Before diving in, ensure you have the following tools installed:

- **Spark:** spark-3.2.4-bin-hadoop3.X
- **Kafka:** kafka_0-10_2.12
- **ElasticSearch:** elasticsearch-8.8.2
- **Kibana:** kibana-8.8.2
- **Hive:** hive_2.12:3.2.4

## Implementation Steps

### 1. Collecting Real-Time Bike Data

We begin by gathering bike station details in real-time using a dedicated API. The collected attributes include:

- Station numbers, contract names, banking availability
- Stand counts (total and available)
- Bike counts (total and available)
- Station address, status, and geolocation
- Last update timestamp

This data is then streamed to Kafka for further processing.

### 2. Kafka Real-Time Producer

A Kafka producer is set up to ingest data from the bike streaming API and publish it to a dedicated topic. Ensure Kafka and Zookeeper are running and that the topic is correctly configured.

### 3. PySpark Streaming

We utilize Spark Streaming to periodically collect data from Kafka, process it, and then index it into Elasticsearch. This step is crucial for real-time analytics and visualization.

### 4. Indexing Data to Elasticsearch

Ensure Elasticsearch is running and configured to receive and store bike station data. You can verify its status by visiting http://localhost:9200.

### 5. Visualization with Kibana

Kibana serves as the window to our data, offering real-time visualization capabilities. Set it up and explore the data through various visual elements and dashboards. Access Kibana at http://localhost:5601.

### 6. Storing Historical Data in Hive

For long-term storage and historical analysis, we integrate Hive into our pipeline, ensuring that all collected data is retained and readily available for deep dives and trend analysis.

## Running the Pipeline

To get everything up and running, follow these steps:

1. **Start Elasticsearch:**
   ```bash
   sudo systemctl start elasticsearch
   sudo systemctl enable elasticsearch
   ```

2. **Start Kibana:**
   ```bash
   sudo systemctl start kibana
   sudo systemctl enable kibana
   ```

3. **Launch Zookeeper:**
   ```bash
   ./bin/zookeeper-server-start.sh ./config/zookeeper.properties
   ```

4. **Launch Kafka:**
   ```bash
   ./bin/kafka-server-start.sh ./config/server.properties
   ```

5. **Run the Kafka producer:**
   ```bash
   python3 ./real-time-bike-producer.py
   ```

6. **Execute the PySpark consumer:**
   ```bash
   spark-submit --packages [dependencies] /path/to/spark_consumer.py
   ```

## Launching the Kibana Dashboard

To visualize the data:

1. Navigate to http://localhost:5601.
2. Access Management > Kibana > Saved Objects.
3. Import `Real-Time-Bike_Project-Project-Dashboard.ndjson`.
4. Explore the dashboard.

## Initializing the Hive Table

To set up Hive for historical data storage:

1. Start Hadoop with `start-dfs.sh`.
2. Initiate the Hive metastore service.
3. Visit http://localhost:50070 to manage and interact with your data.
