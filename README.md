# API Monitoring with Prometheus and Beyla

This project provides a stack for monitoring APIs using **Prometheus** and **Beyla** (Grafana's auto-instrumentation tool). The setup includes:

- A sample API service (`testapi`).
- Beyla for automatic instrumentation of the API to collect metrics.
- Prometheus to scrape and store the metrics.

## Table of Contents
- [Overview](#overview)
- [Services](#services)
- [Setup](#setup)
- [Configuration](#configuration)
- [Accessing the Services](#accessing-the-services)
- [License](#license)

## Overview

The architecture consists of:
- **testapi**: A sample API service that runs on port `5070`.
- **Beyla**: A lightweight tool that automatically instruments the API and exposes metrics in Prometheus format.
- **Prometheus**: Scrapes metrics from Beyla and provides a storage backend for analysis and visualization.

## Services

```yaml
version: "3.8"

services:
  # Exemplos de serviços de API
  testapi:
    image: testapi:1.0
    ports:
      - "5070:5070"

  # Serviço Beyla para `testapi`
  autoinstrumenter_testapi:
    image: grafana/beyla:latest
    pid: "service:testapi"
    privileged: true
    environment:
      BEYLA_OPEN_PORT: 5070
      BEYLA_PROMETHEUS_PORT: 9091 # Porta para expor métricas

  # Serviço Prometheus
  prometheus:
    image: prom/prometheus:latest
    ports:
      - "9090:9090"
    volumes:
      - ./prometheus.yml:/etc/prometheus/prometheus.yml

```
## Setup

### Prerequisites
- **Docker** and **Docker Compose** installed on your machine.

### Steps to Run

### 1. **Clone this repository**
   ```bash
   git clone https://github.com/username/api-monitoring.git
   cd api-monitoring
   ```

### Step 2: **Start the stack**

After cloning the repository, start the services using Docker Compose:

```bash
docker-compose up -d
```

### 3. **Verify the services are running**

Check if the services are up and running with the following command:

```bash
docker ps
```

### 4. **Access the services**

Once the stack is running, you can access the following services:

- **testapi**: [http://localhost:5070](http://localhost:5070)
- **Prometheus**: [http://localhost:9090](http://localhost:9090)

### Step 5: Stopping the Services

To stop all running services, use the following command:

```bash
docker-compose down
```

### 6. **Viewing the logs**

If you want to view the logs for all services, use:

```bash
docker-compose logs
docker-compose logs testapi
```

