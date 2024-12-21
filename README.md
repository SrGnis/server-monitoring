# Server Monitoring System

A simple monitoring system built with Grafana, Mimir, Loki, Alloy, and Docker Compose.

## Overview

This repository contains the configuration files for a fully functional monitoring system. The system uses Alloy to scrape metrics and logs from nodes and Docker Compose to simplify deployment.

## Components

* **Grafana**: A visualization platform for displaying metrics and logs from various sources.
* **Mimir**: A query engine for querying stored data.
* **Loki**: A logging and metric storage system.
* **Alloy**: A metrics and logs scraper.

## Usage

### Prerequisites

Make sure you have Docker and Docker Compose installed on your system before proceeding.

### Quick Start

To launch the monitoring system without any preconfiguration, simply run:

```bash
docker-compose --profile all up -d
```

This will start all services in the background. You can access the Grafana interface at `http://localhost:3000`.

### Detailed Configuration

The Docker Compose service configuration files are located in their respective directories.

* **Mimir** and **Loki**: Basic configuration files.
* **Grafana**:
	+ `config`: Basic configuration file.
	+ `definitions`: Defaults dashboards for Grafana. See [Grafana Dashboards](https://grafana.com/grafana/dashboards/) for more options.
	+ `datasources`: Defines data sources for Loki and Mimir.
	+ `plugins`: Configuration for enblade plugins.
* **Alloy**:
	+ `endpoints`: Location of Mimir and Loki.
	+ `config.alloy`: Scraping configuration.

#### Launching individual services

To launch individual services, modify the following files with new host information:

* `alloy/endpoints.json`
* `grafana/provisioning/datasources.yaml`

#### Scraping multiple servers

To scrape metrics from multiple servers, you can launch the Alloy service on a separate server using:

```bash
docker-compose --profile alloy up -d
```

Before launching the Alloy service, modify the `alloy/endpoints.json` file to point to the Mimir and Loki services. Additionally, update the `alloy/config.alloy` file to specify which scraping tasks you want to perform.

## Considerations

* The `alloy` service is running in `host` mode and has mounted the host root to scrape all system information.
* To allow `alloy` to read all log files, set read permissions on the `/var/log` directory for the `473` user (alloy user) using:

```bash
sudo setfacl -R -d -m u:473:r /var/log
sudo setfacl -R -m u:473:r /var/log
```

* The services come configured without authentication; consider modifying these configurations in production.