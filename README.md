<div align="center">

# 🌱 AgriNova AI
### Intelligent Crop Disease Early Warning, Multilingual Advisory & Regional Surveillance Platform

[![FastAPI](https://img.shields.io/badge/FastAPI-0.111.0-009688?style=for-the-badge&logo=fastapi&logoColor=white)](https://fastapi.tiangolo.com)
[![React](https://img.shields.io/badge/React-19.0-61DAFB?style=for-the-badge&logo=react&logoColor=black)](https://react.dev)
[![Vite](https://img.shields.io/badge/Vite-6.0-646CFF?style=for-the-badge&logo=vite&logoColor=white)](https://vitejs.dev)
[![Tailwind CSS](https://img.shields.io/badge/Tailwind_CSS-3.4-38B2AC?style=for-the-badge&logo=tailwind-css&logoColor=white)](https://tailwindcss.com)
[![PyTorch](https://img.shields.io/badge/PyTorch-2.0+-EE4C2C?style=for-the-badge&logo=pytorch&logoColor=white)](https://pytorch.org)
[![Google Gemini](https://img.shields.io/badge/Google_Gemini-1.5_Flash-4285F4?style=for-the-badge&logo=google&logoColor=white)](https://ai.google.dev)
[![Supabase](https://img.shields.io/badge/Supabase-PostgreSQL_%26_Storage-3ECF8E?style=for-the-badge&logo=supabase&logoColor=white)](https://supabase.com)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg?style=for-the-badge)](LICENSE)

<p align="center">
  <b>Bridging the gap between smallholder farmers and agriculture extension officers through computer vision, localized LLM advisories, and geospatial outbreak tracking.</b>
</p>

[Key Features](#-key-features) •
[System Architecture](#-system-architecture) •
[Tech Stack](#-tech-stack) •
[Quick Start](#-quick-start) •
[Environment Variables](#-environment-variables) •
[API Reference](#-api-endpoints-reference) •
[Deployment](#-deployment)

</div>

---

## 📖 Overview

**AgriNova AI** is an end-to-end intelligent agricultural health platform built to detect, triage, and control crop disease outbreaks before they escalate into regional crises.

Smallholder farmers can simply take a photo of an afflicted crop leaf to receive **instant disease classification**, an **OpenCV-powered lesion severity score**, and **culturally contextualized treatment advice in Sinhala, Tamil, or English**. Concurrently, regional **Agriculture Officers** monitor an interactive command dashboard featuring real-time outbreak clustering, automated triage queues for low-confidence or high-severity cases, field inspection workflows, and an AI-driven Agricultural Copilot.

---

## ✨ Key Features

### 👨‍🌾 For Farmers
- 📸 **Instant Leaf Diagnosis**: Upload crop photos for rapid disease classification powered by **EfficientNet-B0** (PlantVillage 38-class compatible).
- 🔬 **Lesion Severity Quantification**: Computer vision color-space analysis (HSV & OpenCV) measures necrotic tissue percentage and categorizes severity (*Low, Moderate, Severe*).
- 🗣️ **Multilingual Generative Advisory**: Actionable, step-by-step treatment guidance (chemical, organic/cultural, preventive) tailored in **Sinhala (සිංහල)**, **Tamil (தமிழ்)**, and **English** using Google Gemini.
- ⛅ **Microclimate Risk Integration**: Correlates live weather indicators (temperature, humidity, precipitation from Open-Meteo) with disease spread risk.
- 🔔 **Early Warning Broadcasts**: Push alerts notifying farmers when an outbreak is detected within a 25 km radius of their land.

### 🏛️ For Agriculture Extension Officers
- 🗺️ **Geospatial Outbreak Heatmap**: Interactive Leaflet map displaying real-time cluster radiuses, active hot-spots, and spatial disease distributions.
- 🎫 **Automated Triage & Ticket System**: Auto-generates high-priority review tickets when AI confidence is borderline (`< 0.70`), severity is critical, or high-risk pathogens are detected.
- 🚜 **Field Visit Dispatch & Workflow**: Complete lifecycle tracking for on-site field visits (Schedule $\to$ En-Route $\to$ Completed with GPS and verification notes).
- 🤖 **AI Officer Assistant**: LLM-backed decision copilot equipped with regional context to answer technical agricultural queries, treatment efficacies, and quarantine protocols.
- 🔄 **Continuous AI Feedback Loop**: Officer corrections and laboratory verifications are logged to curate retraining datasets for model refinement.

---

## 🏗️ System Architecture

```mermaid
flowchart TD
    subgraph Client ["Client Layer (Frontend)"]
        FP["👨‍🌾 Farmer Portal\n(Photo Upload & Advisory)"]
        OD["🏛️ Officer Dashboard\n(Tickets, Map & Outbreaks)"]
    end

    subgraph Gateway ["API Gateway (FastAPI)"]
        API["FastAPI App Server\n(:8000)"]
    end

    subgraph AI_Engine ["AI & Analytics Pipeline"]
        CL["EfficientNet-B0\n(Disease Classifier)"]
        SE["OpenCV / HSV\n(Lesion Severity Estimator)"]
        LLM["Google Gemini 1.5 Flash\n(Multilingual Advisory & Copilot)"]
        OS["Outbreak Clustering Service\n(Spatial Radius Scan)"]
    end

    subgraph External ["External Services"]
        WX["Open-Meteo API\n(Weather Risk)"]
        SB_DB[("Supabase PostgreSQL\n(Reports, Tickets, Farms)")]
        SB_ST["Supabase Storage\n(crop-images bucket)"]
    end

    FP -->|POST /api/reports| API
    OD -->|GET /api/officer/*| API

    API --> CL
    API --> SE
    API --> LLM
    API --> OS
    API --> WX

    API --> SB_DB
    API --> SB_ST
```

---

## 🛠️ Tech Stack

| Domain | Technology | Description |
|---|---|---|
| **Frontend Framework** | **React 19** + **Vite 6** | Modern, lightning-fast reactive web UI |
| **Styling & Icons** | **Tailwind CSS** + **Lucide Icons** | Dark-themed agricultural glassmorphism interface |
| **Geospatial Mapping** | **Leaflet** + **React-Leaflet** | Dynamic cluster mapping & coordinate plotting |
| **Backend API** | **FastAPI** + **Uvicorn** | Asynchronous Python REST backend with OpenAPI/Swagger |
| **Computer Vision / AI** | **PyTorch** + **Torchvision** | EfficientNet-B0 deep neural network for leaf disease scoring |
| **Image Processing** | **OpenCV** + **Pillow** + **NumPy** | HSV mask color analysis for necrotic tissue quantification |
| **LLM & Advisory** | **Google Gemini 1.5 Flash** | Multilingual advisory engine & Officer Copilot |
| **Database & Storage** | **Supabase** (PostgreSQL + S3 Storage) | Relational persistence, Row-Level Security & image bucket |
| **Weather Engine** | **Open-Meteo API** | Free weather forecast API for humidity and temperature telemetry |

---

## 📁 Repository Structure

```
AgriNova_AI/
├── backend/
│   ├── app/
│   │   ├── ai/               # Classifier, HSV severity, LLM client, advisor
│   │   ├── models/           # SQLAlchemy / Pydantic schema models
│   │   ├── routers/          # FastAPI routes (reports, analysis, officer, weather)
│   │   ├── schemas/          # Request & response data transfer objects
│   │   ├── services/         # Business logic (outbreaks, tickets, notifications)
│   │   ├── config.py         # App configuration & settings validation
│   │   └── main.py           # FastAPI entry point
│   ├── .env.example          # Backend environment variables template
│   ├── requirements.txt      # Python dependencies
│   ├── run_cli_test_suite.py # Automated end-to-end CLI test suite
│   └── test_e2e_scenarios.py # Scenario test runner
├── frontend/
│   ├── src/
│   │   ├── components/       # UI Components (Navbar, Leaflet Map, Cards, Badges)
│   │   ├── pages/            # Farmer & Officer pages
│   │   ├── services/         # Axios API clients
│   │   └── App.jsx           # React application routes
│   ├── .env.example          # Frontend environment variables template
│   ├── package.json          # Node.js dependencies & scripts
│   └── vite.config.js        # Vite configuration
├── supabase/
│   └── migration.sql         # Supabase database schema, tables & RLS setup
├── .env.example              # Consolidated monorepo environment template
├── .gitignore                # Production Git ignore rules
├── Procfile                  # Process file for Heroku / Render deployment
├── render.yaml               # Render Cloud deployment blueprint
├── requirements.txt          # Root Python pointer
└── start-dev.bat             # One-click Windows local development runner
```

---

## 🚀 Quick Start

### Prerequisites
- **Python 3.10+** (Python 3.11 recommended)
- **Node.js 18+** & **npm**
- A free **Supabase** account ([supabase.com](https://supabase.com))
- A **Google Gemini API Key** ([aistudio.google.com](https://aistudio.google.com))

---

### 1. Database & Storage Setup (Supabase)

1. Create a new project on [Supabase](https://supabase.com).
2. Go to the **SQL Editor** in your Supabase dashboard and run the entire script in [`supabase/migration.sql`](file:///c:/Users/Maleesha%20Maddegoda/Desktop/AgriNova_AI-main/AgriNova_AI-main/supabase/migration.sql).
3. Under **Storage**, ensure the public bucket `crop-images` is created (or run the snippet below in the SQL Editor):
   ```sql
   INSERT INTO storage.buckets (id, name, public)
   VALUES ('crop-images', 'crop-images', true)
   ON CONFLICT DO NOTHING;
   ```
4. Note down your **Project URL**, **Anon Key**, **Service Role Key**, and **Postgres Connection String**.

---

### 2. Backend Setup (FastAPI)

```bash
# Navigate to the backend directory
cd backend

# Create and activate virtual environment
python -m venv venv

# Windows (PowerShell):
.\venv\Scripts\Activate.ps1
# Windows (CMD):
.\venv\Scripts\activate.bat
# macOS / Linux:
source venv/bin/activate

# Install dependencies
pip install -r requirements.txt

# Configure environment variables
cp .env.example .env     # macOS/Linux
copy .env.example .env   # Windows

# Start FastAPI server
uvicorn app.main:app --reload --port 8000
```

- **Backend API**: [http://localhost:8000](http://localhost:8000)
- **Interactive Swagger Docs**: [http://localhost:8000/docs](http://localhost:8000/docs)

---

### 3. Frontend Setup (React + Vite)

```bash
# Open a new terminal and navigate to the frontend directory
cd frontend

# Configure environment variables
cp .env.example .env     # macOS/Linux
copy .env.example .env   # Windows

# Install packages
npm install

# Start Vite development server
npm run dev
```

- **Farmer Portal**: [http://localhost:5173](http://localhost:5173)
- **Officer Command Center**: [http://localhost:5173/officer](http://localhost:5173/officer)

---

### ⚡ One-Click Local Development (Windows)

If you are on Windows, simply double-click [`start-dev.bat`](file:///c:/Users/Maleesha%20Maddegoda/Desktop/AgriNova_AI-main/AgriNova_AI-main/start-dev.bat) or run:

```bat
start-dev.bat
```
This automatically verifies dependencies, starts the backend on port `8000`, and launches the Vite frontend on port `5173` in separate terminals.

---

## 🔐 Environment Variables

### Backend (`backend/.env`)

| Variable | Description | Example / Default |
|---|---|---|
| `SUPABASE_URL` | Supabase project REST URL | `https://xyz.supabase.co` |
| `SUPABASE_KEY` | Supabase Anon Public Key | `eyJhbG...` |
| `SUPABASE_SERVICE_KEY` | Supabase Service Role Secret Key | `eyJhbG...` |
| `DATABASE_URL` | Direct PostgreSQL connection string | `postgresql://postgres:...@db...` |
| `STORAGE_BUCKET` | Supabase image storage bucket | `crop-images` |
| `MODEL_PATH` | Path to PyTorch model weights (`.pth`) | `./models/plant_disease_efficientnet.pth` |
| `DEV_MODE` | If `true`, returns mock AI scores if weights are absent | `false` |
| `GEMINI_API_KEY` | Google Gemini API Key for advisory and officer assistant | `AIzaSy...` |
| `LLM_MODEL` | Gemini / OpenAI model version | `gemini-1.5-flash` |
| `WEATHER_API_URL` | Open-Meteo weather endpoint | `https://api.open-meteo.com/v1/forecast` |
| `OUTBREAK_RADIUS_KM`| Clustering radius for disease outbreak surveillance | `25` |
| `OUTBREAK_WINDOW_DAYS`| Day window for historical outbreak scans | `14` |
| `PORT` | FastAPI server port | `8000` |
| `FRONTEND_URL` | Allowed origin for CORS middleware | `http://localhost:5173` |

### Frontend (`frontend/.env`)

| Variable | Description | Default |
|---|---|---|
| `VITE_API_URL` | Base URL for FastAPI backend | `http://localhost:8000` |

---

## 📡 API Endpoints Reference

| Method | Endpoint | Description |
|---|---|---|
| `POST` | `/api/reports/` | Submit a crop leaf photo and metadata for diagnosis |
| `GET` | `/api/reports/{id}` | Retrieve diagnosis status, confidence, and advisory |
| `GET` | `/api/weather/risk` | Get microclimate risk analysis based on coordinates |
| `GET` | `/api/officer/tickets` | List triage tickets (low confidence, high severity) |
| `PATCH`| `/api/officer/tickets/{id}` | Update ticket priority, assignment, or status |
| `GET` | `/api/officer/outbreaks` | Get active regional disease clusters and coordinates |
| `POST` | `/api/officer/assistant` | Query the AI Agricultural Copilot with officer questions |
| `POST` | `/api/officer/visits` | Schedule and manage agricultural officer field visits |
| `POST` | `/api/officer/feedback` | Submit ground-truth confirmation or diagnosis correction |

---

## 🧪 Testing & Quality Assurance

Run the automated CLI verification suite to test full diagnosis workflows, officer endpoints, and integration health:

```bash
cd backend
python run_cli_test_suite.py
```

To run end-to-end integration scenarios:
```bash
cd backend
python test_e2e_scenarios.py
```

---

## 🚢 Deployment

### Backend (Render / Railway / Heroku)
The repository includes a ready-to-deploy [`render.yaml`](file:///c:/Users/Maleesha%20Maddegoda/Desktop/AgriNova_AI-main/AgriNova_AI-main/render.yaml) blueprint and [`Procfile`](file:///c:/Users/Maleesha%20Maddegoda/Desktop/AgriNova_AI-main/AgriNova_AI-main/Procfile).
- Set root directory or run command: `uvicorn main:app --host 0.0.0.0 --port $PORT`
- Add all required environment variables in your cloud provider settings.

### Frontend (Vercel / Netlify)
The frontend is configured with [`frontend/vercel.json`](file:///c:/Users/Maleesha%20Maddegoda/Desktop/AgriNova_AI-main/AgriNova_AI-main/frontend/vercel.json) for single-page application routing rewrites.
- Set Root Directory to `frontend`
- Build Command: `npm run build`
- Output Directory: `dist`
- Set `VITE_API_URL` to your production backend URL.

---

## 👥 Authors & Acknowledgments

Developed with ❤️ for **CodeArena'26 (Topic 05: Early Warning for Crop Disease Across a Region)**.

---

## 📄 License

This project is licensed under the [MIT License](LICENSE).
