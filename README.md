## Weather ETL Pipeline
A daily Apache Airflow DAG that extracts weather data from the Open-Meteo API, transforms it, and loads it into a PostgreSQL database.

## Overview
This ETL pipeline fetches current weather data for London and stores it in a PostgreSQL database. The pipeline runs daily and follows a simple Extract-Transform-Load pattern using Airflow's TaskFlow API.

## Features
- Extract: Fetches current weather data from Open-Meteo API
- Transform: Processes and structures the weather data
- Load: Stores the data in PostgreSQL with automatic table creation
- Scheduled: Runs daily using Airflow's scheduling capabilities
- Error Handling: Includes basic error handling for API failures

## Data Pipeline Flow
- Extract Weather Data: Calls the Open-Meteo API to get current weather information
- Transform Weather Data: Structures the API response into a clean format
- Load Weather Data: Inserts the transformed data into PostgreSQL

## Configuration
Location Settings
The pipeline is currently configured for London:

Latitude: 51.5074
Longitude: -0.1278
To change the location, update the LATITUDE and LONGITUDE variables at the top of the DAG file.

## Database Schema
This pipeline creates a "weather_data" table with the following structure:

sql
CREATE TABLE weather_data (
    latitude FLOAT,
    longitude FLOAT,
    temperature FLOAT,
    windspeed FLOAT,
    winddirection FLOAT,
    weathercode INT,
    timestamp TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

## Prerequisites
Airflow Providers
Ensure you have the following Airflow providers installed:

apache-airflow-providers-http
apache-airflow-providers-postgres
Airflow Connections
You need to configure two connections in your Airflow instance:

1. PostgreSQL Connection
Connection ID: postgres_default
Connection Type: Postgres
Host: Your PostgreSQL host
Database: Your database name
Username: Your database username
Password: Your database password
Port: 5432 (or your custom port)

2. Open-Meteo API Connection
Connection ID: open_meteo_api
Connection Type: HTTP
Host: https://api.open-meteo.com
Installation
Place the DAG file in your Airflow DAGs directory
Configure the required connections in Airflow UI
The DAG will automatically appear in your Airflow dashboard
Schedule
Schedule: Daily (@daily)
Start Date: Yesterday (to allow immediate execution)
Catchup: Disabled

## Data Collected
The pipeline collects the following weather metrics:

Temperature (°C)
Wind speed (km/h)
Wind direction (degrees)
Weather code (WMO weather interpretation code)
Location coordinates
Timestamp of data insertion
Usage
Once deployed, the DAG will:

Run automatically every day
Fetch the latest weather data for the configured location
Store the data in your PostgreSQL database
Provide logs and monitoring through the Airflow UI
Monitoring
Monitor the pipeline through the Airflow UI:

Check DAG runs for success/failure status
View task logs for debugging
Monitor data quality and pipeline performance
Customization
Change Location
Update the LATITUDE and LONGITUDE constants to fetch weather data for a different location.

Add More Weather Parameters
The Open-Meteo API supports additional parameters. Modify the API endpoint and transformation logic to include more weather data points.

Change Schedule
Modify the schedule parameter in the DAG definition to run at different intervals (e.g., @hourly, @weekly).

Error Handling
The pipeline includes basic error handling:

API request failures will raise exceptions and fail the task
Database connection issues will be logged
Failed runs can be retried through the Airflow UI
Contributing
Fork the repository
Create a feature branch
Make your changes
Test the DAG in your Airflow environment
Submit a pull request

## License
This project is open source and available under the MIT License.

