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

## Task 3: Anomaly Detection Analysis

### Detected Anomalies
1. **High Latency & Resource Spike**
   * **Metrics:** Response time > 1000ms, CPU > 85%, Memory > 85%.
   * **Log:** `ERROR` - High latency and resource exhaustion detected during transaction processing.
2. **Database Timeout Failure**
   * **Metrics:** Normal/moderate metrics.
   * **Log:** `ERROR` - Database connection pool timeout during billing execution.

### Accuracy Assessment
* **Missed Anomalies:** None. All records violating static thresholds or emitting `ERROR` logs were captured.
* **False Positives:** None. Routine `INFO` entries with baseline metrics were correctly classified as normal.

### Limitations & Improvements
* **Limitation:** Static thresholding fails to detect subtle multi-metric drift or dynamic load spikes.
* **Improvement:** Implement dynamic thresholding (e.g., moving-window standard deviation) or isolation forests to detect contextual anomalies automatically.

these are findings for task 3
======================== test session starts =========================
collected 1 item (or more)

tests/test_aiops_pipeline.py .                                [100%]

========================= 1 passed in 0.05s =========================


## Task 5: Workflow Investigation and Corrections

### Problem 1: Broken Module Imports
* **Component Affected:** `src/aiops_pipeline.py`
* **Cause:** Bare imports (`from anomaly_detector import AnomalyDetector`) caused `ModuleNotFoundError` when executing as a module from the root directory.
* **Correction:** Updated imports to use package paths or set `PYTHONPATH=src` during module execution.
* **Verification:** Execution via `PYTHONPATH=src python -m src.aiops_pipeline` runs without import errors.

### Problem 2: Event Topic Name Mismatch
* **Component Affected:** `EventProducer` and `EventConsumer` in `src/aiops_pipeline.py`
* **Cause:** The producer published events to `"service-events"` while the consumer was listening on `"anomaly-events"`.
* **Correction:** Updated both producer and consumer to use the unified `"anomaly-events"` topic name.
* **Verification:** Both components interact with the same topic stream.

### Problem 3: Disconnected Event Topic Instances
* **Component Affected:** `EventTopic` in `src/aiops_pipeline.py`
* **Cause:** `producer` and `consumer` were instantiated with separate `EventTopic` objects, keeping events isolated in separate memory queues.
* **Correction:** Created a single shared `EventTopic("anomaly-events")` instance and passed it to both `EventProducer` and `EventConsumer`.
* **Verification:** Terminal execution confirms `Anomalies detected: 2` and `Events consumed: 2`.

## Task 6: End-to-End Pipeline Execution

### Workflow Verification Checklist
- [x] **1. Operational Data Processed:** Read 10 operational telemetry records from `data/service_data.json`.
- [x] **2. Anomalous Behavior Detected:** Identified 2 records violating threshold & error condition rules.
- [x] **3. Anomaly Event Generated:** Structured event payload created with service metadata and violation reasons.
- [x] **4. Event Published:** `EventProducer` successfully published events to the topic.
- [x] **5. Event Consumed:** `EventConsumer` retrieved all 2 events from the shared `EventTopic("anomaly-events")`.
- [x] **6. Event Processed Successfully:** End-to-end traversal completed without loss or error.
- [x] **7. Final Output Verification:** Correctly reported resource exhaustion (high CPU/memory/latency) and database connection timeout anomalies.