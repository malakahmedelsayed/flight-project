# ✈️ Flight Radar 2024 - Real-Time Flight Tracking Pipeline

This project simulates a **real-time flight tracking system** (similar to AirRadar 2024) using a modern data engineering pipeline.  
It generates fake flight events, streams them through Kafka, processes with Spark, stores in PostgreSQL, and visualizes with Streamlit.

---

## 🛠 Tools & Technologies

- **Python (Faker)** → Generate fake flight data (Producer)  
- **Kafka** → Message broker for real-time streaming  
- **Spark (Structured Streaming)** → Process and transform flight events  
- **Postgres** → Store flight data  
- **Streamlit** → Interactive dashboard to visualize flight positions & statuses  
- **Docker Compose** → Containerized setup for all services  

---


## ▶️ How to Run

1. **Start the environment**
   ```bash
   docker-compose up -d
   ```

2. **Check containers**
   ```bash
   docker ps
   ```

3. **Run the data producer**
   - Open the `script/producer.ipynb` notebook in **Anaconda environment**  
   - Run it to generate fake flights data and send events to Kafka topic `flights`.

4. **Verify Kafka topic**
   - Open **Kafka UI** in your browser  at [http://localhost:8090](http://localhost:8090)  
   - Check that topic `flights` is receiving messages  

5. **Check PostgreSQL**
   - Open **pgAdmin** at [http://localhost:8085](http://localhost:8085)  
   - Login with:  
     - Email: `admin@admin.com`  
     - Password: `admin`  

6. **Create PostgreSQL Connection**
   - In pgAdmin, create a new server connection:
     - **Name**: `postgres_general`  
     - **Host**: `postgres`  
     - **Port**: `5432`  
     - **Username**: `admin`  
     - **Password**: `admin`  

7. **Create Database**
   - Open **Query Tool** in pgAdmin and run:
     ```sql
     CREATE DATABASE flights_project;
     ```

8. **Create Flights Table**
   - Connect to the `flights_project` database and run:
     ```sql
     CREATE TABLE flights (
         flight_id VARCHAR PRIMARY KEY,
         origin TEXT,
         destination TEXT,
         status TEXT,
         departure_time BIGINT,
         arrival_time BIGINT
     );
     ```

9. **Run Spark Script**
   - Open the `scripts` folder and copy the Spark script into a Jupyter Notebook.  
   - Access Jupyter at [http://localhost:8888](http://localhost:8888).  
   - Before running the script, install PostgreSQL driver:
     ```bash
     pip install psycopg2-binary
     ```
   - Run the notebook and confirm that records are being inserted into PostgreSQL.  

10. **View Dashboard**
   - Open the Streamlit app at [http://localhost:8501](http://localhost:8501)  
   - Explore the **real-time flights map** and **flight statuses**.  

---

## 📊 Data Flow

```
[ Python Producer (Faker) ]
            ↓
     Kafka Topic: flights
            ↓
   Spark Structured Streaming
            ↓
        PostgreSQL
            ↓
     Streamlit Dashboard
```
---

!['pipeline.png](./image/Data%20Pipeline.jpg)

## 📝 Example Data

```json
{
  "flight_id": "FL1001",
  "origin": [40.6413, -73.7781],
  "destination": [51.4700, -0.4543],
  "status": "On Time",
  "departure_time": 1695638400,
  "arrival_time": 1695645600
}
```

---

## 🧹 Clean up

To stop and remove containers, volumes, and networks:

```bash
docker-compose down -v
```

---

## 📌 Notes

- Kafka topic: **`flights`**  
- Postgres DB: **flights_project**  
- Streamlit UI shows flight map with status colors (On Time / Delayed / Cancelled).  
- All services are containerized and orchestrated via **Docker Compose**.

- <img width="1910" height="940" alt="لقطة شاشة 2025-10-16 041449" src="https://github.com/user-attachments/assets/c1a3d9e7-7295-44da-9b8e-d377fa2da8da" />

