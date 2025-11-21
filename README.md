# operation

# SMS Checker Application Cluster Operation

This repository contains the necessary configuration files and documentation for running and operating the SMS Checker application cluster. The cluster consists of two main services: `sms-app` (Java/Spring Boot API Gateway) and `sms-model` (Python/Flask ML Model).

## Requirements

1. Docker & Docker Compose: Must be installed and running on the host machine.

2. Built Images: The following application images must be built and available in the local Docker registry:

    - `sms-app` (from the `app` repository)

    - `sms-model` (from the `model-service` repository)

    To build these images, navigate to their respective source directories and run:
    
    ```
    docker build -t sms-app .
    docker build -t sms-model .
    ```

## Running the Application Cluster

The application is started using `docker compose`. It defaults to using port `8080` for the app frontend and `8081` for the model service, as defined in the configuration.

### Standard Startup

To run the application with default settings (App Frontend on `8080`, Model on `8081`):
```
docker compose up -d
```

### Accessing the Application

Once both services are healthy, the web UI can be accessed at:
http://localhost:8080/sms

### Flexible Configuration

The services are configured to be flexible using environment variables. You can override the default ports by setting them in your terminal session before running docker compose.


| Variable  | Service  | Default  | Description  |
|---|---|---|---|
| `SERVER_PORT`  | `sms-app`  | `8080`  | Port the app service will listen on for the user browser.  |
| `MODEL_PORT`  | `sms-model`  | `8081`  | Port the model service will listen on for the frontend requests.  |


### Example: Running the Frontend on port 9000 and the Model on port 8090:

```
SERVER_PORT=9000 MODEL_PORT=8090 docker compose up -d
```



(After running the example, access the app at http://localhost:9000/sms)

## Stopping the Cluster

To stop and remove the running containers:
```
docker compose down
```

