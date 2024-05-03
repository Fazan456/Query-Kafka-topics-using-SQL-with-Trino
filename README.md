# Query-Kafka-topics-using-SQL-with-Trino
In here We're going to see how to query kafka topics using SQL with Trino.

First let's have and introduction on our key components.

* **Trino**: The powerful SQL query engine we'll use to interrogate data directly from Kafka.
* **Kafka**: Our data streaming platform, handling the flow of social media posts.
* **Zookeeper**: Works behind the scenes to manage Kafka's cluster state and configurations.
* **Redpanda** Console: A web-based UI for visualizing and managing our Kafka data streams.
* **PostgreSQL**: Our relational database, used to demonstrate data storage and retrieval

# Prerequisites
* **docker and docker compose**
* **python 3.x**

# Step1:  Installation and Setup

**1) Install dependencies**
```bash
pip install .
```
This command installs all required Python libraries, including the Kafka client and Faker for data generation.
**2) Launch with docker compose**
```bash
docker-compose up -d
```
This starts with all the necessary services: Kafka, Zookeeper, Trino, Redpanda Console, and PostgreSQL.

# Step 2: Simulating Social Media Data
Now, let's generate some data for a fictional social network, main.py script uses Faker to create these simulated social media posts.

**1) Single batch:** 
To send a fixed number of posts to Kafka, run:
```bash
kafka-producer demo.fake_social_media --count 20 
```

In here Kafka producer  produce 10 data records to a Kafka topic named "demo.fake_social_media." It's a simple command used for demonstrating or testing Kafka producers in batch mode.

**2) Continuous Mode:**
Mimic a live stream by sending posts continuously, run:
```bash
kafka-producer demo.fake_social_media --continuous
```

In here  Kafka producer  continually generate and send data to the specified topic,It's a simple command used for demonstrating or testing Kafka producers in continuous(stream) mode.

> [!NOTE] 
> It's crucial to use the topic name demo.fake_social_media as this is the topic for which Trino configuration is set. 
> This can be found in the `trino/etc/kafka/demo.fake_social_media.json` and `trino/etc/catalog/kafka.properties files.

# Step 3: Observing Kafka in Action
To see data flowing, let's see it in action in Redpanda Console:
1.Open Redpanda Console: open http:localhost:8080 in local browser.
2.Explore Kafka topics: Navigate to the topic you've been publishing to and observe the incoming messages.

# Step 4: Querying with Trino
Time to query our data using Trino:
1. **Access Trino:** Use the Trino CLI or SQL client with the following configuration:

   - **JDBC URL:** `jdbc:trino://localhost:8080`
   - **Username:** `admin`
   - **Password:** No password is required for this demo; leave the field blank.
   - **Driver Name:** `Trino`. The Trino driver can be downloaded from [Trino's Driver Maven Repository](https://repo1.maven.org/maven2/io/trino/trino-jdbc/433/trino-jdbc-433.jar).
  
2. **Run a Basic Query:** Start with a simple select statement:

```SQL
SELECT
	username,
	post_content,
	likes,
	comments,
	shares,
	timestamp
FROM
	kafka.demo.fake_social_media

```

We can explore the details of your request on Trino UI to `http://localhost:8000` on your browser. 

3. **Data Transfer to PostgreSQL:** Execute a query to transfer data to PostgreSQL:

```SQL
CREATE TABLE postgres.public.social_media AS
SELECT
	username,
	post_content,
	likes,
	comments,
	shares,
	timestamp
FROM
	kafka.demo.fake_social_media
```

4. **Check the data on PostgreSQL table:** Execute this query for example

```SQL
SELECT * FROM postgres.public.social_media
```









   



   
