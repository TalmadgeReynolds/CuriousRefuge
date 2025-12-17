

# NBP → Kling Turbo Filmmaking Engine
### API-Driven AI Filmmaking Pipeline with Project-Level Cost, Prompt & Asset Tracking

---

## Overview

This application is a **fully API-driven AI filmmaking system** designed for professional, repeatable, and auditable video generation.

The core workflow:

> **Nano Banana Pro (NBP)** generates high-fidelity still frames →  
> **Kling 2.5 Turbo** animates those frames into cinematic 1080p video →  
> Everything is orchestrated through **FastAPI**, visualized in a **React dashboard**,  
> and tracked **project-by-project** with full cost, prompt, and asset lineage.

The system is designed to function like a **mini production studio backend**, enabling:
- shot-based filmmaking
- transparent cost tracking
- reproducibility
- client-ready reporting
- exports suitable for invoices, approvals, and audits

---

## Key Capabilities

### 🎬 Shot-Based Filmmaking
- Each project contains multiple shots
- Each shot supports:
  - Single-frame → motion
  - Start → End frame interpolation
  - Multi-frame interpolation (up to 7 frames)
- Each shot produces:
  - NBP stills
  - Kling video outputs
  - Full prompt + parameter history

---

### 🗂 Project-Level Tracking (Core Feature)

All work is organized under **Projects**.

A project acts as a top-level container for:
- shots
- prompts
- assets
- costs
- logs
- exports

This enables:
- per-project cost accounting
- client billing transparency
- clean archival
- spreadsheet export

---

### 💰 Cost Tracking (Granular & Auditable)

Every billable action creates a **cost event**:
- NBP image generation
- Kling video generation
- reruns / retries
- optional storage or processing steps

Costs are logged in a **ledger model**, not inferred.

You can:
- see total cost per shot
- see total cost per project
- export full cost breakdowns for clients

Typical costs:
- NBP image (2K): ~$0.139
- Kling Turbo (10s): ~$0.42
- **Typical shot total: ~$0.70**
- **Typical minute of video: ~$4.20**

This is ~3–5× cheaper than UI-based tools.

---

### 🧾 Prompt Lineage & Reproducibility

Every prompt is logged with:
- model used (NBP or Kling)
- full prompt text
- parameters (resolution, seed, motion mode, duration)
- timestamp
- associated cost
- project + shot association

This enables:
- full reproducibility
- prompt comparison
- prompt iteration tracking
- future fine-tuning or analytics

---

### 📦 Asset Management

All generated assets are stored in **S3** and tracked in the database:
- NBP still frames
- reference images
- Kling video outputs
- intermediate frames (optional)

Each asset is linked to:
- project
- shot
- generation step
- metadata (resolution, duration, model)

---

### 📊 Client-Ready Spreadsheet Export

For any project, the system can generate a **downloadable Excel spreadsheet** containing:

- Project overview
- Shot list
- All prompts used
- Cost breakdown
- Generated assets
- Optional job logs / version history

This is suitable for:
- client delivery
- invoices
- approvals
- archival
- internal audits

---

## System Architecture

React / Next.js Dashboard | v FastAPI Orchestration Layer | v Async Task Queue (Celery / Dramatiq) | v Workers ├─ NBP Image Worker ├─ Kling Turbo Video Worker └─ QC / Validation Worker (optional) | v AWS S3 (Assets) PostgreSQL (Projects, Shots, Costs, Prompts) Redis (Queue / State)
---

## Database Model (High Level)

### project
- id
- name
- description
- status
- created_at

### shot
- id
- project_id
- name
- type (single, start_end, multi_frame)
- duration_seconds
- status

### asset
- id
- project_id
- shot_id
- type (nbp_still, kling_video, reference)
- s3_url
- metadata

### prompt_log
- id
- project_id
- shot_id
- model (nbp | kling)
- prompt_text
- parameters (json)
- cost_usd
- created_at

### cost_ledger
- id
- project_id
- shot_id
- model
- resource_type
- units
- usd_cost
- created_at

### job_log (optional)
- id
- project_id
- shot_id
- event
- details
- timestamp

---

## Folder Structure

/ ├── backend/ │ ├── main.py │ ├── routes/ │ │ ├── projects.py │ │ ├── shots.py │ │ ├── assets.py │ │ ├── exports.py │ ├── models/ │ │ ├── project.py │ │ ├── shot.py │ │ ├── asset.py │ │ ├── prompt_log.py │ │ ├── cost_ledger.py │ ├── workers/ │ │ ├── nbp_worker.py │ │ ├── kling_worker.py │ ├── services/ │ │ ├── nbp_api.py │ │ ├── kling_api.py │ │ ├── spreadsheet_export.py │ │ ├── s3.py │ └── utils/ │ ├── settings.py │ ├── logger.py │ ├── frontend/ │ ├── src/ │ │ ├── pages/ │ │ ├── components/ │ │ ├── hooks/ │ │ └── api/ │ ├── prompts/ │ ├── nbp_templates.md │ ├── kling_start_end.md │ └── README.md
---

## Spreadsheet Export (Client Deliverable)

Each export includes multiple sheets:

### Project Overview
- Project name
- Date range
- Total shots
- Total cost

### Shot List
- Shot name
- Type
- Duration
- Status
- Output filenames
- Cost per shot

### Prompts
- Shot
- Model
- Prompt text
- Parameters
- Cost
- Timestamp

### Assets
- Shot
- Asset type
- Filename
- S3 URL

### Cost Ledger
- Model
- Resource
- Units
- Cost
- Timestamp

---

## Environment Variables

NBP_API_KEY= NBP_API_URL= KLING_API_KEY= KLING_API_URL= AWS_ACCESS_KEY_ID= AWS_SECRET_ACCESS_KEY= AWS_S3_BUCKET= POSTGRES_URL= REDIS_URL=
---

## Running Locally

### Backend
pip install -r requirements.txt uvicorn main:app --reload
### Workers
celery -A workers worker --loglevel=INFO
### Frontend
npm install npm run dev
---

## Deployment

Recommended:
- AWS ECS (workers)
- AWS RDS (PostgreSQL)
- AWS S3 (assets)
- Redis (queue)
- Docker + Terraform

---

## Roadmap

- Batch shot creation
- Timeline / sequence builder
- Automatic transitions
- Multi-model routing (WAN, Veo, Sora)
- Audio generation + sync
- Shot-to-invoice automation

---

## Philosophy

This system is intentionally designed to:
- treat AI like a **production tool**, not a toy
- keep costs visible and controllable
- preserve creative intent
- support client trust and transparency

---

## License

MIT (or project-specific)


