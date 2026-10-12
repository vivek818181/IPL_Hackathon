<div align="center">

# ⚡ Adaptive Energy City

### Intelligent 3D Smart City Energy Management Simulator

[![React](https://img.shields.io/badge/React-19-61DAFB?logo=react&logoColor=white)](https://react.dev)
[![TypeScript](https://img.shields.io/badge/TypeScript-6.0-3178C6?logo=typescript&logoColor=white)](https://www.typescriptlang.org)
[![Three.js](https://img.shields.io/badge/Three.js-r186-000000?logo=threedotjs&logoColor=white)](https://threejs.org)
[![FastAPI](https://img.shields.io/badge/FastAPI-Python-009688?logo=fastapi&logoColor=white)](https://fastapi.tiangolo.com)
[![Scikit-learn](https://img.shields.io/badge/Scikit--learn-ML-F7931E?logo=scikit-learn&logoColor=white)](https://scikit-learn.org)
[![Vite](https://img.shields.io/badge/Vite-8-646CFF?logo=vite&logoColor=white)](https://vitejs.dev)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

**A fully functional, browser-based simulation combining an interactive 3D digital-twin city with machine learning–powered energy forecasting, intelligent demand response scheduling, and real-time grid telemetry — all running locally with zero external API dependencies.**

[Features](#-features) · [Demo](#-quick-start) · [Architecture](#-architecture) · [ML Models](#-machine-learning-pipeline) · [Tech Stack](#-technology-stack) · [Contributing](#-contributing)

</div>

---

## 📖 Overview

Modern urban power grids face three critical challenges: **peak demand shocks** (the "Duck Curve"), **renewable energy intermittency**, and **information asymmetry** between grid operators and consumers. **Adaptive Energy City** tackles all three with:

- 🏙️ A **3D interactive city** (Three.js + React Three Fiber) where you can click buildings, trigger power events, and watch energy flows in real time
- 🤖 **Gradient Boosting ML models** (15 pipelines across 5 building archetypes) for 24-hour demand, price-peak, and shortage-risk forecasting
- ⚙️ A **priority-aware greedy scheduling engine** that automatically shifts flexible loads to optimal time slots
- 📡 **Real-time Server-Sent Events (SSE)** streaming live grid telemetry to resident dashboards
- 🔄 A **hybrid resilience architecture** — the frontend runs standalone if the ML backend is offline

---

## ✨ Features

### 🏙️ City Admin Dashboard
| Feature | Description |
|---|---|
| **3D Digital Twin** | Interactive Three.js city — drag, zoom, rotate, and click buildings to inspect energy data |
| **2D Network View** | Toggle to an SVG network diagram with animated energy flow lines |
| **Live Dashboard** | 8 KPI metric cards + 4 real-time Recharts (demand, solar, price, cost) |
| **Event Simulation** | Trigger solar drops, price spikes, grid reductions, and demand surges |
| **Scheduling Engine** | Priority-aware greedy optimizer with deadline tracking and cost savings |
| **Decision Panel** | Plain-language AI explanations for every scheduling decision |
| **Building Inspector** | Per-building energy data, device list, and capacity utilization bar |
| **Before/After View** | Side-by-side scheduling results with cost savings in ₹ |
| **Power Shortage Demo** | One-click scenario that creates and resolves a shortage automatically |
| **Welcome Onboarding** | Guided intro modal for first-time users |

### 🏠 Resident Energy Dashboard
| Feature | Description |
|---|---|
| **Personal Portal** | Scoped view showing only the resident's assigned building |
| **Live Consumption** | Real-time gauges, kW readings, and tariff indicators via SSE |
| **Shortage Alerts** | Proactive toast notifications before predicted grid overloads |
| **Appliance Schedules** | View and approve automated load-shifting recommendations |
| **Savings Tracking** | Personal cost savings charts and energy optimization history |
| **Privacy Isolation** | Zero cross-tenant data leakage — strict backend boundary scoping |

---

## 🚀 Quick Start

### Prerequisites
- **Node.js** ≥ 18
- **Python** ≥ 3.10 (for ML backend)

### Option 1: Frontend Only (No Python Required)
```bash
# Install dependencies
npm install

# Start the development server
npm run dev

# Open http://localhost:5173
```
> The frontend runs a full in-browser simulation engine — no backend needed for the core experience.

### Option 2: Full Stack (Frontend + ML Backend)
```bash
# 1. Install frontend dependencies
npm install

# 2. Install Python dependencies
pip install -r backend/requirements.txt

# 3. Generate dataset & train ML models (first time only)
python backend/generate_dataset.py
python backend/train_models.py

# 4. Launch everything (Windows PowerShell)
.\start.ps1
```
> The `start.ps1` script automatically starts both servers and opens the Admin + Resident dashboards in separate browser tabs.

---

## 🏗️ Architecture

```
adaptive-energy-city/
├── src/                          # React + TypeScript Frontend
│   ├── components/
│   │   ├── CityCanvas.tsx        # 3D Three.js / React Three Fiber city
│   │   ├── City2DView.tsx        # SVG 2D network fallback view
│   │   ├── Dashboard.tsx         # KPI metrics + 4 Recharts panels
│   │   ├── ControlPanel.tsx      # Simulation controls & event triggers
│   │   ├── BuildingInspector.tsx  # Per-building energy inspector
│   │   ├── DecisionPanel.tsx     # AI decision explanations
│   │   ├── BuildingFocusView.tsx  # Detailed building focus mode
│   │   ├── WelcomeScreen.tsx     # Onboarding modal
│   │   └── pages/               # Shortage alerts & reports pages
│   ├── resident/
│   │   ├── ResidentApp.tsx       # Resident energy dashboard
│   │   └── useResidentStream.ts  # SSE stream hook for real-time data
│   ├── engine/
│   │   └── scheduler.ts         # In-browser scheduling engine (96 slots)
│   ├── store/
│   │   └── simStore.ts          # Zustand state store with recalc logic
│   ├── data/
│   │   └── cityData.ts          # Buildings, devices, base time slots
│   └── types/
│       └── energy.ts            # TypeScript types & interfaces
│
├── backend/                      # Python ML Backend
│   ├── api.py                   # FastAPI server (10 REST + SSE endpoints)
│   ├── generate_dataset.py      # Synthetic 8,760h dataset generator
│   ├── train_models.py          # ML training pipeline (15 models)
│   ├── models/                  # Trained .pkl model files + metrics.json
│   ├── data/                    # Generated CSV datasets
│   └── requirements.txt         # Python dependencies
│
└── start.ps1                    # One-command full-stack launcher
```

### System Flow

```
┌─────────────────────────────────────────────────────────┐
│                    React Frontend (Vite)                 │
│                                                         │
│  ┌─────────────┐  ┌──────────────┐  ┌───────────────┐  │
│  │ 3D City     │  │  Dashboard   │  │   Resident    │  │
│  │ (Three.js)  │  │  (Recharts)  │  │   Portal      │  │
│  └──────┬──────┘  └──────┬───────┘  └───────┬───────┘  │
│         │                │                   │          │
│  ┌──────┴────────────────┴───────────────────┘          │
│  │           Zustand Store + Local Scheduler            │
│  └──────────────────────┬───────────────────────────────┘
│                         │
│            REST API + SSE Stream
│                         │
│  ┌──────────────────────┴───────────────────────────────┐
│  │              FastAPI ML Backend (:8000)               │
│  │                                                       │
│  │  ┌──────────────┐  ┌────────────┐  ┌──────────────┐  │
│  │  │   Demand     │  │ Price-Peak │  │  Shortage    │  │
│  │  │  Forecaster  │  │ Classifier │  │   Risk       │  │
│  │  │  (GBR ×5)    │  │ (GBC ×5)   │  │  (GBC ×5)    │  │
│  │  └──────────────┘  └────────────┘  └──────────────┘  │
│  └───────────────────────────────────────────────────────┘
```

---

## 🤖 Machine Learning Pipeline

### Three Models × Five Buildings = 15 Trained Pipelines

| Model | Algorithm | Purpose |
|---|---|---|
| **Demand Forecaster** | `GradientBoostingRegressor` | 24-hour ahead hourly kW demand prediction |
| **Price-Peak Classifier** | `GradientBoostingClassifier` | Predicts expensive tariff windows (≥ ₹14/kWh) |
| **Shortage-Risk Classifier** | `GradientBoostingClassifier` | Predicts grid overload probability (Low/Med/High) |

### Feature Engineering (9 Dimensions)
`hour` · `day_of_week` · `month` · `is_weekend` · `temperature_c` · `solar_factor` · `demand_lag_1h` · `demand_lag_24h` · `rolling_mean_24h`

### Model Performance

| Building | Model MAE | Baseline MAE | Improvement | Shortage AUC |
|---|:---:|:---:|:---:|:---:|
| City Hospital | **2.27 kW** | 4.18 kW | **+45.7%** | 0.989 |
| Smart Factory A | **2.82 kW** | 13.84 kW | **+79.7%** | 0.993 |
| Office Tower | **1.06 kW** | 8.01 kW | **+86.7%** | — |
| Shopping Mall | **1.80 kW** | 6.26 kW | **+71.2%** | 0.999 |
| Green Residences | **0.80 kW** | 1.74 kW | **+54.2%** | — |

> *Training uses strict chronological splits (70/15/15) — no data leakage.*

---

## 🏢 Simulated City

| Building | Type | Grid Limit | Solar Capacity |
|---|---|:---:|:---:|
| 🏥 City Hospital | Hospital | 120 kW | 20 kW |
| 🏭 Smart Factory A | Industrial | 100 kW | 30 kW |
| 🏢 Sunrise Office Tower | Commercial | 90 kW | 25 kW |
| 🛒 Galaxy Shopping Mall | Retail | 150 kW | 40 kW |
| 🏘️ Green Residences | Residential | 60 kW | 15 kW |
| ☀️ Solar Farm | Generation | 200 kW | 150 kW |
| ⚡ District Substation | Distribution | 500 kW | — |
| 🔌 Public EV Hub | EV Charging | 80 kW | 10 kW |

---

## 🎮 Simulation Controls

| Control | Effect |
|---|---|
| ▶️ **Start / Pause** | Begin or pause the 24-hour simulation |
| 🔄 **Reset** | Restore all buildings to initial state |
| ⏭️ **+15 min** | Manually advance one time slot |
| 🎚️ **Time Slider** | Jump to any point in the simulated day |
| ⚡ **Speed 0.5× – 8×** | Adjust simulation clock speed |
| ☀️ **Solar Drop** | Reduce solar generation by 30% per click |
| 💰 **Price Spike** | Multiply electricity price by +0.5× per click |
| 🔌 **Reduce Grid** | Reduce feeder capacity by 80 kW per click |
| 📈 **More Demand** | Add 50 kW of extra city load per click |
| ✅ **Restore Normal** | Reset all conditions to baseline |

---

## 🛠️ Technology Stack

### Frontend
- **React 19** + **TypeScript 6** — component architecture with full type safety
- **Vite 8** — blazing-fast build tool with HMR
- **Three.js** + **@react-three/fiber** + **@react-three/drei** — 3D rendering
- **Recharts** — interactive real-time charts
- **Zustand** — lightweight state management
- **Framer Motion** — smooth animations and transitions
- **Lucide React** — modern icon system
- **React Router DOM** — client-side routing

### Backend
- **Python 3.10+** + **FastAPI** — async REST API with auto-generated Swagger docs
- **Scikit-learn** — Gradient Boosting ML models
- **Pandas** + **NumPy** — time-series data processing
- **Joblib** — model serialization
- **Uvicorn** — ASGI server with SSE support

---

## 📂 API Endpoints

The FastAPI backend exposes 10 production endpoints at `http://localhost:8000`:

| Method | Endpoint | Description |
|---|---|---|
| `GET` | `/api/buildings` | List all buildings with metadata |
| `GET` | `/api/forecast/{building_id}` | 24h demand + shortage forecast |
| `POST` | `/api/schedule` | Run ML-powered appliance scheduling |
| `GET` | `/api/tariff` | Current time-of-use tariff schedule |
| `GET` | `/api/resident/{id}/building` | Resident's assigned building info |
| `GET` | `/api/resident/{id}/forecast` | Scoped resident forecast |
| `GET` | `/api/resident/{id}/schedule` | Resident's appliance schedule |
| `GET` | `/api/resident/{id}/alerts` | Shortage alerts for resident |
| `GET` | `/api/resident/{id}/stream` | SSE real-time telemetry stream |
| `GET` | `/docs` | Interactive Swagger API documentation |

---

## ⚠️ Disclaimer

This is a **planning and educational simulator** built for a hackathon. It does not calculate real voltage, current, electrical losses, or protection safety margins. Energy data is synthetically generated. Do not use for actual electrical engineering decisions.

---

## 📄 License

This project is open source and available under the [MIT License](LICENSE).

---

<div align="center">

*Built for the IPL Hackathon — Adaptive Energy City Project*

**⭐ Star this repo if you found it useful!**

</div>
