# ELT Pipeline with Docker, Airflow, dbt & PostgreSQL

This repository contains a **custom Extract, Load, Transform (ELT)** project that demonstrates a modular data pipeline using **Docker, PostgreSQL, Airflow, dbt**, and **cron jobs**. It showcases how multiple containers and tools can work together to simulate a real-world data engineering workflow.

---

## 🚀 Project Overview

The pipeline:

* Extracts data from a source PostgreSQL database
* Loads it into a destination PostgreSQL database
* Applies transformations using **dbt**
* Uses **cron jobs** and **Airflow** for automation and orchestration

---

## 📁 Repository Structure

```
.
├── airflow/                  # Airflow DAGs and configs
├── custom_postgres/         # Custom PostgreSQL configs (if any)
├── elt_script/
│   ├── Dockerfile           # Python environment for ELT
│   └── elt_script.py        # Python script that performs ELT
├── logs/                    # Log files
├── source_db_init/
│   └── init.sql             # SQL script to seed source PostgreSQL
├── Dockerfile               # Base Dockerfile
├── docker-compose.yaml      # Docker Compose setup
├── elt.sh                   # Shell script to trigger ELT
├── start.sh                 # Starts the containers
├── stop.sh                  # Stops and cleans up
└── README.md
```

---

## ⚙️ How It Works

### 1. **Container Orchestration**

Using `docker-compose.yaml`, three services are deployed:

* `source_postgres`: PostgreSQL container initialized with sample data
* `destination_postgres`: Empty PostgreSQL container (ELT target)
* `elt_script`: Runs Python-based ELT process

### 2. **ELT Process**

* **Extract**: `elt_script.py` connects to `source_postgres` and dumps the database using `pg_dump`
* **Load**: The dump is loaded into `destination_postgres` using `psql`
* **Transform**: dbt models (in `dbt/`) transform the loaded data

### 3. **Database Initialization**

* `init.sql` initializes the `source_postgres` container with tables:

  * `users`, `films`, `categories`, `actors`, and relationships
* Includes sample data for simulation

---

## ⏱️ Automation with Cron

A **cron job** is configured to run the ELT script at scheduled intervals:

* Keeps the destination database updated automatically
* Useful for simulating regular data ingestion cycles

---

## 🌐 Airflow Orchestration

In an extended version, **Apache Airflow** is used to:

* Schedule and monitor ELT jobs
* Chain tasks (e.g., extract → load → transform) with DAGs
* Integrate with `dbt` and external sources (e.g., Aibyte)

---

## 📎 Requirements

* Docker + Docker Compose
* Python 3.x (within containers)
* PostgreSQL client tools (`pg_dump`, `psql`)
* dbt CLI (in future updates)

---

## ▶️ Running the Project

```bash
# Start the pipeline
bash start.sh

# Run the ELT manually
bash elt.sh

# Stop and clean up
bash stop.sh
```


