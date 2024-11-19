### Fire Risk Monitoring System

This repository contains the Fire Weather Index (FWI) System developed by Group 12. The project aims to calculate the Fire Weather Index (FWI) using real-time weather data for all Italian municipalities. The system provides the FWI for current weather conditions as well as a 5-day forecast.

### Abstract

The project predicts the Fire Weather Index (FWI) for various cities using real-time weather data and forecasts. The system consists of four main components:
- **User Interface (UI)**
- **Data Fetcher**
- **Data Processor**
- **Spark Streaming Processor**

These components communicate through an MQTT message queue.

### Technologies Used

- **Spark Streaming**: Used for processing and analyzing the real-time data streams to compute the Fire Weather Index.
- **MongoDB**: Used as the primary database to store the raw and processed weather data.
- **Docker + Docker Compose**: Used to containerize and orchestrate the deployment of all components and services.
- **Flask** (Python framework): Used to develop the User Interface, providing an interactive web application for users to input and view data.
- **MQTT (Paho-MQTT)**: Used for message queuing and communication between different components of the system.

[System Architecture](images/system_architecture.png)
### Project Structure

The repository is organized as follows:

```
.
├── data_fetcher
│   ├── Dockerfile
│   ├── data_fetcher.py
│   ├── requirements.txt
├── data_processor
│   ├── Dockerfile
│   ├── data_processor.py
│   ├── requirements.txt
│   └── fwi_calculator.py
├── spark_streaming
│   ├── Dockerfile
│   ├── spark_streaming.py
│   ├── requirements.txt
│   └── fwi_calculator.py
├── UserInterface
│   ├── Dockerfile
│   ├── ui_flask.py
│   ├── requirements.txt
│   ├── COMUNI.json
│   ├── templates
│   │   └── ...
│   ├── static
│       └── ...
├── mongo
│   └── ...
├── mosquitto
│   └── ...
├── docker-compose.yml
├── config.yaml
├── requirements.txt
└── README.md
```

### Setup and Configuration

#### Configuration File

The configuration file `config.yaml` includes necessary parameters for API keys from [OpenWeather](https://openweathermap.org/api). Ensure it is correctly set up before running the application.

If you are the professors of the course BDT, check the email with the deliver of the project, the API for you is there!

#### Docker Setup

The project uses Docker Compose to manage and run services. It includes the Docker Compose file docker-compose.yml which sets up MongoDB, Mosquitto (MQTT broker), and other services.

### How to Run

1. **Clean the environment**:
    ```sh
   docker-compose down --remove-orphans    
   ```
2. **Start Docker**:
    ```sh
    docker-compose up --build
    ```

Access the user interface at `http://127.0.0.1:5000`.

### Components Description

#### User Interface (UI)

- **Files**:
  - `ui_flask.py`: Main file to run the UI component.
  - `templates`: HTML templates for the UI.
  - `static`: Static files like CSS and JS.
  - `COMUNI.json`: Contains city data.

[Home](images/home.png)
Figure 1: Home page of the Fire Risk Detection System.
[Current](images/current_we.png)
Figure 2: Results for the current time for the city of Trento.
[Forecast](images/forecast_we.png)
Figure 3: Forecast results for the city of Trento. 
#### Data Fetcher

- **Files**:
  - `data_fetcher.py`: Fetches real-time weather and forecast data, publishes to MQTT.
  - `requirements.txt`: Dependencies for data fetcher.

The Data Fetcher subscribes to the `city/select` topic, fetches current and forecasted weather data for the specified city, and publishes the raw data to the `city/data_raw` topic.

#### Data Processor

- **Files**:
  - `data_processor.py`: Processes raw data, calculates Fire Weather Index (FWI), stores in MongoDB, and publishes to MQTT.
  - `fwi_calculator.py`: Contains functions to calculate FWI.
  - `requirements.txt`: Dependencies for data processor.

The Data Processor subscribes to the `city/data_raw` topic, processes the data to compute the FWI, and publishes the processed data to the `city/data_filtered` topic.

#### Spark Streaming

- **Files**:
  - `spark_streaming.py`: Uses Spark for streaming data processing.
  - `fwi_calculator.py`: Contains functions to calculate FWI.
  - `requirements.txt`: Dependencies for Spark streaming.

The Spark Streaming component processes the streaming data to provide real-time updates and predictions.
Note: this system use mock-up data and not actual real time data. 

### To Be Implemented

- **Integration with Real-time Weather Data**:
  - Enhance the `spark_streaming.py` script to integrate with real-time weather data from the OpenWeather API.
  - Overcome limitations of the OpenWeather API free tier to enable more frequent and accurate data requests.
- **Incorporate Dask for Parallel Computing**:
  - Utilize Dask for parallel computing to improve the performance and scalability of data processing tasks within the system.
- **Accurate and Timely Fire Risk Assessments**:
  - Provide more accurate fire risk assessments based on actual weather conditions by integrating real-time data sources.
- **Additional Features**:
  - Implement more sophisticated data models and prediction algorithms to enhance the accuracy of the Fire Weather Index predictions.
  - Develop automated monitoring and error recovery mechanisms to ensure robustness and reliability of the system.

### Dependencies

- **Docker** v24.0.2
- **Docker Compose** v2.18.1
- **Python** v3.11.0
- **Flask** v2.3.2
- **Paho-MQTT** v1.6.1
- **PyYAML** v5.4.1
- **NumPy** v1.21.0
- **Requests** v2.25.1
- **MongoDB**
- **Mosquitto**

Ensure all dependencies are installed and correctly set up to run the project successfully.

---
### Authors
This project was created by group 12, consisting of:
 - Agnese Cervino - [@AgneCer](https://github.com/AgneCer)
 - Alessandra Gandini - [@alegandini](https://github.com/alegandini)
 - Gaudenzia Genoni - [@Ggenoni](https://github.com/Ggenoni)



---
### Need some help?
If you are still a student, like us, it is not always easy to navigate the world of Big Data. You may encounter some issues running this project, but we've got your back. Here are some helpful tips to check if everything is running correctly, step by step. Do not despair: one day you will be able to code a system like this, maybe even better!

##### IS MQTT WORKING?

With the following code, you can subscribe to topics, send messages, and check if everything is working.

Access to container Mosquitto
```
   docker exec -it [name_of_container_mosquitto] /bin/sh
```
Subscribe to the topic you want to check
```
   mosquitto_sub -h localhost -t "city/select"
   mosquitto_sub -h localhost -t "city/data_raw"
   mosquitto_sub -h localhost -t "city/data_filtered"
   mosquitto_sub -h localhost -t "city/synthetic_data"
```

##### HOW DO I ACCESS MONGODB?

1. Access the MongoDB Container
```sh
docker exec -it [name_of_container_mongo] mongosh
```
2. Check the Databases
```sh
show dbs
```
3. Select the Database: 
```sh
use data_db
# OR
use weather_db
```
4. Show Available Collections:
```sh
show collections
```
5. View Data in a Collection: 
```sh
db.data_raw.find().pretty()
# OR
db.weather_data.find().pretty()
```

##### ARE CONTAINERS WORKING?

Check the status of containers:
```
docker ps -a
```
Check the logs of a specific container
```
docker-compose logs -f [container_name]
```


##### IS THE CODE INSIDE CONTAINER WORKING?

Access to the powershell of the container
```
docker run -it [container_name] /bin/sh
```
Run the code
```
python [program].py
```
