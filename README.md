
# 🌦 Weather ETL Pipeline with Airflow, Docker & Postgres

![Airflow](https://img.shields.io/badge/Airflow-Orchestration-blue?logo=apacheairflow)
![Docker](https://img.shields.io/badge/Docker-Containerization-blue?logo=docker)
![Postgres](https://img.shields.io/badge/PostgreSQL-Database-blue?logo=postgresql)
![Python](https://img.shields.io/badge/Python-ETL-yellow?logo=python)
![DBeaver](https://img.shields.io/badge/DBeaver-DB_Tool-green?logo=dbeaver)

🚀 An **ETL pipeline** to fetch live weather data from the [Open Meteo API](https://open-meteo.com/), transform it, and load it into a **PostgreSQL database** using **Apache Airflow** running on **Astro + Docker**.

---

## 📊 Architecture

```mermaid
flowchart LR
    A[🌐 Open Meteo API] -->|Extract| B[Airflow Task: extract_weather_data]
    B -->|Transform JSON → Dict| C[Airflow Task: transform_weather_data]
    C -->|Load into Postgres| D[(PostgreSQL weather_data table)]
    D -->|View Data| E[DBeaver UI]
```

---

## 🎯 Features

* ✅ Automated daily ETL pipeline with **Airflow**
* ✅ Extracts weather data (temperature, windspeed, direction, weather code)
* ✅ Transforms into structured format
* ✅ Loads into **PostgreSQL** table (`weather_data`)
* ✅ Query, monitor & debug via **DBeaver**
* ✅ Dockerized environment for portability

---

## 🛠️ Tools & Tech Stack

* **Apache Airflow (Astro)** → Orchestrating ETL
* **Docker** → Containerized Postgres + Airflow
* **PostgreSQL** → Weather data storage
* **Python** → ETL scripting
* **DBeaver** → Query and visualization

---

## ⚙️ Setup Guide

### 1️⃣ Clone the repository

```bash
git clone https://github.com/Harshitraiii2005/weather-etl-airflow.git
cd weather-etl-airflow
```

### 2️⃣ Start PostgreSQL with Docker Compose

```yaml
version: '3'
services:
  postgres:
    image: postgres:13
    container_name: postgres_db
    environment:
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: postgres
      POSTGRES_DB: postgres
    ports:
      - "5432:5432"
    volumes:
      - postgres_data:/var/lib/postgresql/data

volumes:
  postgres_data:
```

Run it:

```bash
docker-compose up -d
```

---

### 3️⃣ Setup Astro Airflow

```bash
astro dev init   # initialize Astro Airflow project
astro dev start  # start Airflow locally
```

UI available at → `http://localhost:8080`

---

### 4️⃣ Configure Connections in Airflow

* `postgres_default` → PostgreSQL DB
* `open_meteo_api` → Base URL: `https://api.open-meteo.com`

---

### 5️⃣ Airflow DAG (ETL)

```python
from airflow import DAG
from airflow.providers.http.hooks.http import HttpHook
from airflow.providers.postgres.hooks.postgres import PostgresHook
from airflow.decorators import task
from datetime import datetime, timedelta
...
```

👉 DAG runs **daily**, extracts → transforms → loads into Postgres.

---

## 🗄 PostgreSQL Schema

```sql
CREATE TABLE IF NOT EXISTS weather_data (
    latitude FLOAT,
    longitude FLOAT,
    temperature FLOAT,
    windspeed FLOAT,
    winddirection FLOAT,
    weathercode INT,
    timestamp TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

---

## 🖥️ Screenshots

### ✅ DAG Success Run
<img width="1366" height="728" alt="Airflow - Google Chrome 9_4_2025 10_56_08 PM" src="https://github.com/user-attachments/assets/a62689f6-cd4e-4f46-8f85-10e1af2d40d5" />


### 📊 DAG Overview

<img width="1366" height="728" alt="Airflow - Google Chrome 9_4_2025 10_58_10 PM" src="https://github.com/user-attachments/assets/87c2ef75-3d39-4fd7-b700-15f9f84059ea" />


### 🛢 PostgreSQL Schema in DBeaver

<img width="1366" height="685" alt="DBeaver 25 2 0 - weather_data 9_4_2025 11_09_03 PM" src="https://github.com/user-attachments/assets/99f5817f-961b-43fe-a2d5-04649edff466" />




## 🔍 Queries in PostgreSQL

```sql
-- Fetch latest weather entry
SELECT * FROM weather_data ORDER BY timestamp DESC LIMIT 5;

-- Average temperature in last 7 days
SELECT AVG(temperature) FROM weather_data
WHERE timestamp >= NOW() - INTERVAL '7 days';
```

---

## 📌 Next Improvements

* Add **historical weather API ingestion**
* Add **dbt or Spark for transformations**
* Build a **dashboard (Grafana/Metabase)** for visualization


