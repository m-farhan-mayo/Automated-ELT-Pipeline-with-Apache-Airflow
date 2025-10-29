# Automated-ELT-Pipeline-with-Apache-Airflow



This repository contains the code for a complete, automated ELT (Extract, Load, Transform) data pipeline built and orchestrated using Apache Airflow.

The project demonstrates the core concepts of Airflow by building a DAG (Directed Acyclic Graph) that performs a real-world task:

Extracts data from a public API (e.g., Weather API, Random User API, or any REST source).

Loads the raw, unprocessed data into a staging area or data lake (like a local file system, S3, or a staging table in a database).

Transforms the raw data from the staging area into a clean, structured, and analytics-ready format.

Loads the final, transformed data into a production data warehouse (e.g., PostgreSQL, Snowflake, or BigQuery).

This entire workflow is defined as code, scheduled to run automatically (e.g., daily), and is fully monitored through the Airflow UI.

Key Concepts & Project Recall (What I Learned)
This project serves as a hands-on-guide to the fundamental components of Apache Airflow:

DAGs (Directed Acyclic Graphs): The core of Airflow. This is the Python file that defines the entire workflow, including its schedule, tasks, and dependencies.

Recall: You defined a DAG object, setting its dag_id, start_date, schedule_interval (e.g., '@daily' or a cron expression), and catchup=False.

Operators: These are the "what gets done." Each operator is a template for a single task. This project used:

@task Decorator / PythonOperator: The most flexible operator. Used to write custom Python functions for the Extract and Transform steps (e.g., call the API, parse JSON, clean data).

BashOperator: Used for running simple shell commands, like echo or perhaps curl for the initial data fetch.

PostgresOperator / SqliteOperator: Used to run SQL commands directly against a database, perfect for the final Load step (e.g., TRUNCATE and INSERT or a MERGE statement).

EmptyOperator: Used as placeholder tasks to define the start and end of the pipeline for clarity.

Task Dependencies: Defining the order of the workflow.

Recall: You used the bit-shift operators (>> and <<) to set the "downstream" and "upstream" relationships. For example: start >> fetch_data >> process_data >> load_to_warehouse >> end.

Airflow UI: The web-based interface used for managing and monitoring the pipeline.

Recall: You used the UI to:

Trigger the DAG manually.

Monitor the status of DAG runs (success, failed, running).

Check the Logs for a specific task to debug errors.

View the workflow in the "Grid" and "Graph" views.

Connections & Variables: The secure, production-safe way to manage credentials and configuration.

Recall: You stored your API keys, database passwords, and login information in the Airflow UI (in the Admin -> Connections section) instead of hard-coding them in the DAG file. You used Variables (Admin -> Variables) to store non-secret values like file paths or API endpoints.

XComs (Cross-Communications): The "magic" that lets tasks share small pieces of information.

Recall: The Extract task "pushed" the file path of the saved raw data, and the Transform task "pulled" that file path so it knew what file to read and process.
