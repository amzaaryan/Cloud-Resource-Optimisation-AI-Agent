# Autonomous AI Agent for Cloud Resource Optimization

An end-to-end dynamic resource auto-scaling framework powered by time-series workload forecasting and container management APIs. This project monitors microservices, predicts oncoming traffic spikes, and autonomously adjusts container replica counts to balance operational costs against Service Level Agreements (SLAs).

---

## 📌 System Architecture & Workflow

           ┌────────────────────────┐
           │  Locust Traffic Load   │
           │       Generator        │
           └───────────┬────────────┘
                       │ (HTTP Load Spikes)
                       ▼
           ┌────────────────────────┐
           │ Dockerized Microservice│
           └───────────┬────────────┘
                       │ (Container Metrics)
                       ▼
           ┌────────────────────────┐
           │  Prometheus Monitoring │
           └───────────┬────────────┘
                       │ (PromQL Telemetry Stream)
                       ▼
┌────────────────────────────────────────────────────────┐
│                   AI AGENT ENGINE                      │
│                                                        │
│   ┌──────────────────────┐    ┌────────────────────┐   │
│   │ Workload Predictor   │ ──►│ Scaling Decision   │   │
│   │ (LSTM / ML Model)    │    │ Engine             │   │
│   └──────────────────────┘    └─────────┬──────────┘   │
└─────────────────────────────────────────┼──────────────┘
                                          │ (Scale Commands)
                                          ▼
                              ┌────────────────────────┐
                              │   Docker Engine API    │
                              └───────────┬────────────┘
                                          │ (Live Status)
                                          ▼
                              ┌────────────────────────┐
                              │  Streamlit Dashboard   │
                              └────────────────────────┘


---

## 🔀 Application Flowchart

                      [ START ]
                          │
                          ▼
            Fetch Metrics from Prometheus
               (CPU, RAM, Request Rate)
                          │
                          ▼
             Predict Workload (T + 5 min)
                 Using ML / LSTM Model
                          │
                          ▼
             Is Predicted Load > 75%?
              ├─── YES ───► Action: SCALE_UP (+1 Replica)
              │
              └─── NO  ───► Is Predicted Load < 20%?
                             ├─── YES ───► Action: SCALE_DOWN (-1 Replica)
                             │
                             └─── NO  ───► Action: HOLD (No Change)
                          │
                          ▼
             Execute Action via Docker API
                          │
                          ▼
             Update Streamlit Dashboard
                          │
                          ▼
              Wait Cooldown Interval (30s)
                          │
                          └───► (Repeat Loop)

---

## 🛠️ Tech Stack

* **Infrastructure & Monitoring:** Docker, Docker Compose, Prometheus, cAdvisor
* **Workload Generator:** Locust (Python)
* **Machine Learning Engine:** Python, Pandas, Scikit-Learn / PyTorch
* **Control Loop & API:** FastAPI, Docker SDK for Python
* **Frontend Visualization:** Streamlit

---

## 📁 Repository Structure

```text
cloud-ai-optimizer/
│
├── docker-compose.yml           # Environment deployment file
├── requirements.txt             # Dependencies
│
├── workload_generator/
│   └── locustfile.py            # Traffic generation script
│
├── metrics_collector/
│   ├── prometheus.yml           # Telemetry scrape configuration
│   └── collector.py             # Prometheus query client
│
├── ai_agent/
│   ├── model.py                 # Time-series predictor
│   ├── policy.py                # Decision-making rules
│   └── executor.py              # Docker scaling executor
│
├── dashboard/
│   └── app.py                   # Streamlit live monitoring dashboard
│
└── main.py                      # Master control loop entry point
