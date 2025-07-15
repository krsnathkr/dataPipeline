# Sales & Customer Analytics Data Pipeline

This project is an end-to-end data pipeline designed to process raw sales and customer data into a structured analytics warehouse. It leverages a modern data stack with dbt and Apache Spark, following the Medallion architecture to ensure data quality and reliability.

The primary goal is to create a single source of truth for business intelligence, enabling faster reporting and data-driven decision-making.

## Tech Stack

*   **Data Transformation:** dbt (Data Build Tool)
*   **Data Processing:** Apache Spark
*   **Orchestration:** Python
*   **Language:** SQL, Python
*   **Architecture:** Medallion Architecture, Dimensional Modeling
*   **Version Control:** Git

## Project Architecture

This pipeline follows the Medallion architecture, which logically organizes data into three layers:

*   **Bronze:** Raw, unfiltered data ingested from source systems.
*   **Silver:** Cleaned, validated, and enriched data, serving as a single source of truth.
*   **Gold:** Aggregated and business-ready data marts, optimized for analytics and reporting.

This project also uses dbt snapshots to capture changes in mutable source data over time, creating Type 2 Slowly Changing Dimensions for entities like `customer` and `product`.

## Getting Started

### Prerequisites

*   **Python 3.8+**: Required to run dbt and the `main.py` orchestration script.
*   **Java Development Kit (JDK)**: Apache Spark runs on the JVM, so a JDK (e.g., version 8, 11, or 17) is required for Spark to function.
*   **Apache Spark**: Access to a running Spark instance. This can be a local installation or a remote cluster.

### Installation

1.  **Clone the repository:**
    ```bash
    git clone <your-repo-url>
    cd dataPipeline
    ```

2.  **Set up a Python virtual environment (Recommended):**
    ```bash
    python -m venv .venv
    source .venv/bin/activate  # On Windows use `.venv\Scripts\activate`
    ```

3.  **Install Python dependencies:**
    The primary dependency is the `dbt-spark` adapter, which includes `dbt-core` and `pyspark`.
    ```bash
    pip install dbt-spark
    ```

4.  **Set up your dbt profile:**
    dbt requires a `profiles.yml` file in your `~/.dbt/` directory to connect to Spark. The configuration will vary based on your Spark setup.

    *Example for a local Spark instance:*
    ```yaml
    # in ~/.dbt/profiles.yml
    medallion_dbt_spark:
      target: dev
      outputs:
        dev:
          type: spark
          method: local
          schema: default 
    ```
    For detailed connection options (like Databricks or a standalone cluster), please refer to the [official dbt Spark documentation](https://docs.getdbt.com/reference/warehouse-setups/spark-setup).

### Running the Pipeline

To run the full dbt pipeline, navigate to the dbt project directory and execute the following commands:

```bash
cd medallion_dbt_spark

# Install dbt packages
dbt deps

# Run all models
dbt run

# Test the data quality
dbt test

# Generate snapshots
dbt snapshot
```

Alternatively, the pipeline can be orchestrated via the main Python script:

```bash
python main.py
```

## Project Structure

```
.
├── main.py                 # Main Python script for pipeline orchestration
├── medallion_dbt_spark/    # The dbt project
│   ├── dbt_project.yml     # dbt project configuration
│   ├── models/
│   │   ├── staging/        # Silver layer models
│   │   └── marts/          # Gold layer models (dimensional & fact tables)
│   ├── snapshots/          # dbt snapshots for SCD Type 2
│   └── tests/              # Data quality tests
└── README.md               # This file
```