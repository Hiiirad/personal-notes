# Prometheus

- [Prometheus](#prometheus)
  - [Part 01: The Basics](#part-01-the-basics)
    - [Prometheus Architecture](#prometheus-architecture)
    - [Prometheus Limitation](#prometheus-limitation)
  - [Part 02: Installation and Configuration](#part-02-installation-and-configuration)
    - [Installation Options:](#installation-options)
    - [Configuration](#configuration)
  - [Part 03: Prometheus Data (Data Model for Storing Data + Query Language to Interact with Prometheus Data)](#part-03-prometheus-data-data-model-for-storing-data--query-language-to-interact-with-prometheus-data)
  - [Part 04: Visualization](#part-04-visualization)
  - [Part 05: Collecting Metrics](#part-05-collecting-metrics)
  - [Part 06: Alerting](#part-06-alerting)
  - [Part 07: Advanced Concepts (HA and Federation)](#part-07-advanced-concepts-ha-and-federation)
  - [Part 08: Security](#part-08-security)
  - [Part 09: Client Libraries](#part-09-client-libraries)
  - [Part 10: References](#part-10-references)

---
## Part 01: The Basics
[Prometheus](https://prometheus.io) is an open-source **monitoring** and **alerting** tool. It collects data abou aplications and systems and allows you to **visualize** the data and issue alerts based on the data.

**Monitoring** is an important component of DevOps automation. To manage a robust and complex infrastructure, you need to be able to quickly and easily understant what is happening in your systems.

High Level Use Cases:
- **Metric Collection**: Coolect important metrics about your systems and applications in one place.
- **Visualization**: Build dashboards that provide an overview of the health of your systems.
- **Alerting**: Send you an email or mobile notification when something is broken.

### Prometheus Architecture

Prometheus Architecture Diagram:

![Architecture](Images/architecture.png)

The components of a Prometheus system are:
- **Prometheus Server**: A central server that gathers metrics and makes them available.
- **Exporters**: Agents that expose data about systems and applications for collecting by the Prometheus server.
- **Client Libraries**: Easily turn your custom application into an exporter that exposes metrics in a format Prometheus can consume.
- **Prometheus Pushgateway**: Allows pushing metrics to Prometheus for certain specific use cases. Pushgateway acts as middle-man.
- **Alert Manager**: Sends alerts triggered by metric data.
- **Visualization Tools**: Provide useful ways to view metric data. These are not necessarily part of Prometheus.

Prometheus collects metrics using a **pull model**. This means the Prometheus server pulls metric data from exporters - agents do not push data to the Prometheus server.

### Prometheus Limitation

While Prometheus is a great tool for a variety of use cases, it is important to understand when it is not the best tool:
- **100% Accuracy** (e.g., per-request billing): Prometheus is designed to operate even under failure conditions. This means it will continue to provide data even if new data is not available due to failure and outages. If you need 100% up-to-the-minute accuracy, such as in the case of per request billing, Prometheus may not be the best tool to use.
- **Non Time-Series Data** (e.g., log aggregation): Prometheus is built to monitor time-series metrics, especially data that is numeric. It is not the best choice for collecting more generic types of data such as system logs.

---
## Part 02: Installation and Configuration

### Installation Options:
1. Use pre-compiled binaries from [here](https://prometheus.io/download/).
    ```bash
    #!/bin/bash

    sudo useradd -M -r -s /bin/false prometheus
    sudo mkdir /etc/prometheus /var/lib/prometheus
    # Now download the latest version
    tar xzf PROMETHEUS.tar.gz PROMETHEUS/
    sudo cp PROMETHEUS/{prometheus,promtool} /usr/local/bin/
    sudo chown prometheus:prometheus /usr/local/bin/{prometheus,promtool}
    sudo cp -r PROMETHEUS/{consoles,console_libraries} /etc/prometheus/
    sudo cp PROMETHEUS/prometheus.yml /etc/prometheus/prometheus.yml
    sudo chown -R prometheus:prometheus /etc/prometheus
    sudo chown prometheus:prometheus /var/lib/prometheus
    # Test prometheus: prometheus --config.file=/etc/prometheus/prometheus.yml
    
    # We need to turn prometheus to systemd service
    sudo cat >> /etc/systemd/system/prometheus.service << EOF
    [Unit]
    Description=Prometheus Time Series Collection and Processing Server
    Wants=network-online.target
    After=network-online.target

    [Service]
    User=prometheus
    Group=prometheus
    Type=simple
    ExecStart=/usr/local/bin/prometheus \
        --config.file /etc/prometheus/prometheus.yml \
        --storage.tsdb.path /var/lib/prometheus/ \
        --web.console.templates=/etc/prometheus/consoles \
        --web.console.libraries=/etc/prometheus/console_libraries

    [Install]
    WantedBy=multi-user.target
    EOF
    
    sudo systemctl daemon-reload
    sudo systemctl start prometheus
    sudo systemctl enable prometheus
    # Test prometheus service: sudo systemctl enable prometheus
    # The default port of prometheus is 9090
    ```
2. Build from [source](https://github.com/prometheus).
3. Run with Docker using pre-built [images](https://hub.docker.com/r/prom/prometheus/).

### Configuration



---
## Part 03: Prometheus Data (Data Model for Storing Data + Query Language to Interact with Prometheus Data)


---
## Part 04: Visualization 


---
## Part 05: Collecting Metrics


---
## Part 06: Alerting


---
## Part 07: Advanced Concepts (HA and Federation)


---
## Part 08: Security


---
## Part 09: Client Libraries


---
## Part 10: References

1. [Official Prometheus Documentation](https://prometheus.io/docs/)