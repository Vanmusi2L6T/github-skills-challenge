# GitHub Challenge

<img src="https://octodex.github.com/images/Professortocat_v2.png" align="right" height="200px" />

Hey there!

Your challenge is ready.
Follow the instructions provided for this challenge and complete the required tasks in this repository.

Make sure your work is committed and pushed to your repository before submission.

Good luck!


---

&copy; 2025 GitHub &bull; [Code of Conduct](https://www.contributor-covenant.org/version/2/1/code_of_conduct/code_of_conduct.md) &bull; [MIT License](https://gh.io/mit)

## AIOps Assessment Scenario

### Monitored Service
The monitored service is a **payment-service** that processes payment transactions and handles user billing operations.

### Operational Problem
The operational problem being addressed is detecting service degradation and runtime failures—specifically slow response times, high CPU/memory utilization, and critical error conditions such as database or service timeouts.

### Purpose of AIOps
The purpose of AIOps in this assessment is to automatically ingest telemetry and operational data, detect anomalous patterns and threshold violations, publish corresponding events to event topics, and route them through an automated processing pipeline for rapid incident response and operational monitoring.

## Operational Data Analysis

### 1. Metric Fields
* `response_time_ms`: Measures request latency in milliseconds.
* `cpu_utilization`: Measures CPU usage percentage.
* `memory_utilization`: Measures memory usage percentage.

### 2. Log Fields
* `log_level`: Categorizes message severity (`INFO`, `WARN`, `ERROR`).
* `log_message`: Descriptive text recording operational state or failures.

### 3. Timestamp Usage
* `timestamp`: ISO-8601 formatted datetime string used to sequentially order events, align metric observations with corresponding log messages, and identify temporal patterns or anomalies over time.

### 4. Normal Observations
* Low latency (`response_time_ms` ~100–250ms).
* Moderate resource utilization (`cpu_utilization` < 70%, `memory_utilization` < 75%).
* `log_level` set to `INFO` with routine success messages (e.g., successful transaction processing).

### 5. Unusual / Anomalous Observations
* High latency spikes (`response_time_ms` > 1000ms).
* Elevated resource consumption (`cpu_utilization` > 85%, `memory_utilization` > 85%).
* `log_level` set to `WARN` or `ERROR` with failure descriptions (e.g., connection timeouts, database errors, memory warnings).