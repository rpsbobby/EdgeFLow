# 🧠 EdgeFlow

> **EdgeFlow** is a DevOps-oriented smart camera edge node designed for Raspberry Pi devices.  
> It captures and streams lightweight telemetry or image-based metadata to Kafka, focusing on reliability, observability, and CI/CD best practices — not raw AI inference.

---

## 📦 Project Overview

EdgeFlow represents a modular **edge-to-cloud data pipeline** where each Raspberry Pi device operates as a self-contained service node:

| Layer              | Component           | Description                                                                                                                                    |
|--------------------|---------------------|------------------------------------------------------------------------------------------------------------------------------------------------|
| **Edge Node**      | `edgeflow-node`     | FastAPI-based service that captures frames (stubbed or via Maverick Camera Module 3), emits metadata to Kafka, and exposes Prometheus metrics. |
| **Infrastructure** | `infra-stack`       | Local Docker Compose setup providing Kafka, Prometheus, and Grafana for testing and monitoring.                                                |
| **CI/CD**          | `.github/workflows` | GitHub Actions pipeline for multi-arch builds (ARM / x86), publishing Docker images to GHCR.                                                   |

---

## ⚙️ Features
- **FastAPI service** with `/healthz`, `/metrics`, and `/publish` endpoints.  
- **Kafka integration** via `aiokafka` for telemetry publishing.  
- **Prometheus metrics** (`Counter`, `Summary`, `Gauge`) for observability.  
- **Dockerized build** optimized for Raspberry Pi (ARM64).  
- **CI/CD ready** — GitHub Actions multi-arch build & GHCR push.  
- **Future-proof**: can be extended with OpenCV processing or YOLO inference later.

---

## 🚀 Getting Started

### 1️⃣ Requirements
- Docker & Docker Compose  
- Python ≥ 3.11 (for local testing)  
- Raspberry Pi 4 / 5 with 64-bit OS  
- Optional: Maverick Camera Module 3  

### 2️⃣ Quick Start
```bash
# Clone repo
git clone https://github.com/yourusername/edgeflow.git
cd edgeflow

# Run local Kafka + monitoring stack
docker compose -f infra/docker-compose.yml up -d

# Build and run the edge node locally
cd edgeflow-node
docker build -t edgeflow-node:latest .
docker run --env-file .env --network host edgeflow-node:latest
```
### ️⃣ Test the API
``` bash
curl http://localhost:8080/healthz
curl http://localhost:8080/metrics
curl -X POST http://localhost:8080/publish -d '{"frame_id": "001"}'
```
### 🧰 Environment Variables
Variable	Default	Description
KAFKA_BROKER	localhost:9092	Kafka broker address
KAFKA_TOPIC	edgeflow.frames	Topic for frame metadata
NODE_ID	edge-001	Unique ID for the Pi node
LOG_LEVEL	info	Logging verbosity
## 🧪 Development
### Local run (PyCharm or CLI)
```bash
uvicorn app.main:app --reload --port 8080
```

### Test Kafka manually
``` bash
docker exec -it kafka kafka-console-consumer \
  --bootstrap-server localhost:9092 \
  --topic edgeflow.frames --from-beginning
```
## 📊 Observability Stack

Prometheus scrapes /metrics from each node.

Grafana dashboards visualize frame rates, publish latency, and node uptime.

Future: Edge-to-cloud health reporting over Kafka topics (edgeflow.status).

## 🧭 Roadmap
| Milestone	                     | Status	 | Notes                    |
|--------------------------------|---------|--------------------------|
| Core FastAPI + Kafka producer  | 🔜      | Minimal viable edge node |	
| Prometheus metrics	            | 🔜      | 	/metrics endpoint       |
| CI/CD (GitHub Actions + GHCR)	 | 🔜	     | Multi-arch builds        |
| OpenCV capture pipeline	       | 🔜	     | Optional image metadata  |
| Helmfile/K8s deployment	       | 🔜	     | Cloud orchestration      |