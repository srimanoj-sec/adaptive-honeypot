# 🍯 PRAETOR: Research-Grade Stateful Adaptive Honeypot

[![Python 3.11+](https://img.shields.io/badge/python-3.11%2B-blue.svg)](https://www.python.org/)
[![FastAPI](https://img.shields.io/badge/backend-FastAPI-009688.svg)](https://fastapi.tiangolo.com/)
[![Docker](https://img.shields.io/badge/container-Docker%20Compose-2496ED.svg)](https://www.docker.com/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

**PRAETOR** is a stateful adaptive honeypot leveraging a machine learning ensemble (Random Forest, XGBoost, Isolation Forest) and a Contextual Multi-Agent Reinforcement Learning (CMARL) Q-learning engine to dynamically optimize deception environments in real time.

---

## 📌 Core Architecture

```mermaid
flowchart TB
    subgraph Attacker["Attacker Telemetry Ingestion"]
        SSH[SSH Honeypot]
        HTTP[HTTP Honeypot]
        Telnet[Telnet Honeypot]
    end

    subgraph Analytics["Analytics & ML Engine"]
        Class[ML Ensemble: RF + XGBoost]
        Iso[Isolation Forest Anomaly Detector]
        RL[CMARL Reinforcement Learning Agent]
    end

    subgraph Response["Adaptive Deception Engine"]
        Fuser[Decision Engine & State Transition]
        Output[Dynamic Prompt / Slowdown / Honeyfile Response]
    end

    SSH & HTTP & Telnet --> Class & Iso
    Class & Iso --> RL
    RL --> Fuser --> Output
```

---

## 📊 Scientific Benchmark & Empirical Evaluation

### 1. Deception Baseline Comparison

| Environment Profile | Mean Dwell Time (s) | Median Dwell Time (s) | Std Dev | 95% CI |
|---|:---:|:---:|:---:|:---:|
| Static Honeypot | 17.12 | 17.35 | 4.33 | (15.56, 18.67) |
| Random Deception | 27.61 | 25.50 | 10.15 | (23.98, 31.24) |
| No Deception | 2.70 | 2.49 | 1.37 | (2.21, 3.19) |
| **PRAETOR (CMARL)** | **122.83** | **124.36** | **31.84** | **(111.43, 134.22)** |

### 2. Platform Ablation Metrics

| Configured Stack | Mean Threat Intel Points Captured | Degradation Ratio |
|---|:---:|:---:|
| **Full Architecture** | **68.35** | **0.0%** |
| Without Behavior Graph | 47.25 | 30.87% |
| Without Threat Intelligence | 38.65 | 43.45% |
| Without CMARL Engine | 20.80 | 69.57% |

### 3. ML Classifier Efficacy

Trained on a synthetic benchmark dataset (16,000 samples; 9 attack classes; 75/25 train-test split):

| Model | Accuracy | Precision (macro) | Recall (macro) | F1 Score (macro) |
|---|:---:|:---:|:---:|:---:|
| **XGBoost** | **71.9%** | **69.1%** | **66.1%** | **67.3%** |
| Random Forest | 69.4% | 66.5% | 65.9% | 66.0% |
| Isolation Forest (Anomaly) | 77.1% | 90.1% | 75.6% | 82.2% |

---

## 🚀 Quickstart

### 1. Run with Docker

```powershell
# In PowerShell, create a local environment file only if you do not already have one.
if (-not (Test-Path .env)) { Copy-Item .env.example .env }
```

Before starting, replace the example `SECRET_KEY`, `ADMIN_API_KEY`, and
`MANAGEMENT_API_KEY` values in `.env` with unique secrets. Then run:

```bash
docker compose up --build -d
docker compose logs -f praetor-backend
```

The management API and dashboard are available at `http://127.0.0.1:8000`.
The container stores its database and logs in the local `data/` and `logs/`
directories. Stop it with `docker compose down`.

### 2. Run locally with Python

```bash
python -m venv .venv
source .venv/bin/activate  # On Windows: .\.venv\Scripts\Activate.ps1

pip install -r requirements.txt

uvicorn backend.main:app --reload --host 127.0.0.1 --port 8000
```
*Access management docs at `http://127.0.0.1:8000/docs`.*

### 3. Production Isolation Audit Validation

```bash
python scripts/validate_production_isolation.py
```

---

## 📁 Documentation Index

| Document | Purpose |
|---|---|
| [`docs/ARCHITECTURE.md`](docs/ARCHITECTURE.md) | System design, event pipeline, security boundaries |
| [`docs/SECURITY.md`](docs/SECURITY.md) | Security controls, isolation rules |
| [`docs/PRODUCTION_DEPLOYMENT.md`](docs/PRODUCTION_DEPLOYMENT.md) | Docker & VM deployment runbook |
| [`docs/RESEARCH_DOCUMENTATION.md`](docs/RESEARCH_DOCUMENTATION.md) | Full IEEE methodology documentation |

---

## 📜 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
