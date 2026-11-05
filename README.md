# MEDIMIND AI 💊

**Camera-Based AI Medicine Identification & Autonomous Multi-Agent Pharmaceutical Research System**

MEDIMIND AI empowers patients and healthcare professionals to scan medicine packaging, boxes, or blister packs using their device camera, extract legible chemical and dosage attributes via vision AI, and run an autonomous 5-agent research pipeline connected directly to **DailyMed (National Library of Medicine)** and **openFDA** to deliver official clinical monographs with verified citations.

---

## 🔗 Application Access Links

Both services are active and running locally:

| Component | URL | Description |
|---|---|---|
| **Frontend Application** | [http://localhost:5173/](http://localhost:5173/) (or `http://127.0.0.1:5173/`) | React + Tailwind CSS Web & Mobile App |
| **FastAPI Backend** | [http://127.0.0.1:8000/](http://127.0.0.1:8000/) | High-performance Python async backend |
| **Interactive API Documentation** | [http://127.0.0.1:8000/docs](http://127.0.0.1:8000/docs) | Swagger UI for all REST endpoints |
| **Health Check Endpoint** | [http://127.0.0.1:8000/health](http://127.0.0.1:8000/health) | Real-time database & service health status |

---

## 🏛️ Architecture & Multi-Agent Pipeline

MEDIMIND AI coordinates 5 specialized agents that execute in an orchestrator pipeline:

```
[Camera Snapshot / Upload]
            │
            ▼
   1. Vision Agent (OCR & Quality Pre-Check)
            │
            ▼
   2. Identity Agent (Database Match & Resolution)
            │
     ┌──────┴────────────────────────┐
     ▼                               ▼
3. Medicine Info Agent       4. Safety Agent
(openFDA & DailyMed SPL)    (Adverse Effects & Warnings)
     └──────┬────────────────────────┘
            ▼
   5. Verification Agent (Authority & Conflict Audit)
            │
            ▼
[User-Confirmed Official Pharmaceutical Monograph]
```

1. **Vision Agent**: Performs quality inspection (darkness, glare, blur, contrast variance), Optical Character Recognition (OCR), and visual attribute extraction using Gemini Vision.
2. **Identity Agent**: Reconciles extracted packaging attributes against verified drug databases via structured function calling tools (`search_medicine_database`, `resolve_medicine_identity`).
3. **Medicine Information Agent**: Connects live to **openFDA** and **DailyMed** to retrieve official Structured Product Labels (SPL), active ingredients, approved uses, mechanism of action, and manufacturer data.
4. **Safety & Pharmacovigilance Agent**: Audits boxed warnings, contraindications, graded adverse reactions, drug-drug interactions, food/alcohol interactions, special population guidance (pregnancy, pediatric, geriatric), and storage instructions.
5. **Verification Agent**: Audits source authority (DailyMed SPL Set ID, FDA Label ID), flags unverified secondary claims, detects formulation look-alikes, and attaches the clinical non-advice disclaimer.

---

## 🛡️ Clinical Directives & Safety Rules

- **Zero Blind Guesses**: Never identifies a medicine from appearance alone. If the packaging is blurry, dark, or obscured, the system asks for a clearer photo.
- **Mandatory User Confirmation**: Requires user confirmation of the identified candidate before displaying the full product monograph.
- **Verified vs. Unverified Tagging**: Each clinical monograph section is explicitly tagged with `Verified via FDA Label` or `Unverified (Clinical Reference)`.
- **Formulation Look-Alike Protection**: Scrutinizes active ingredient counts and formulations (e.g. Advil vs. Advil Multi-Symptom Cold & Sinus, or Amoxicillin vs. Amoxicillin + Clavulanate) to prevent wrong-medicine assumptions.
- **Official Source Links**: Direct clickable links to the official DailyMed SPL page (`https://dailymed.nlm.nih.gov/...`) and openFDA repository.
- **Non-Personalized Medical Advice Disclaimer**: Clear statement that information is educational and not medical advice or dosage guidance.
- **API Key Security**: Gemini and openFDA keys are kept exclusively on the server in `.env` and never bundled into client-side JavaScript.

---

## 🚀 Setup & Quickstart Instructions

### Prerequisites
- Python 3.12+
- Node.js 18+ & npm
- Device with a webcam or smartphone camera (or image upload)

### 1. Backend Setup (FastAPI)
```powershell
# Navigate to backend directory
cd backend

# Activate virtual environment
.\venv\Scripts\Activate.ps1

# Install dependencies
pip install -r requirements.txt

# Start the FastAPI server with hot-reload
uvicorn app.main:app --host 127.0.0.1 --port 8000 --reload
```
*Backend runs at:* `http://127.0.0.1:8000` (API Docs: `http://127.0.0.1:8000/docs`)

### 2. Frontend Setup (React + Vite)
```powershell
# In a separate terminal, navigate to frontend directory
cd frontend

# Install dependencies
npm install

# Start Vite dev server
npm run dev
```
*Frontend runs at:* `http://localhost:5173`

---

## 🧪 Hackathon Demonstration Walkthrough

1. **Open Frontend**: Navigate to [http://localhost:5173](http://localhost:5173).
2. **Scan or Upload Packaging**:
   - Click **Scan Medicine** to open the camera viewfinder.
   - Or click **Upload Image** to select any medicine packaging image.
3. **Quality & Blur Defense Test**:
   - Upload an obscured or blurry image.
   - Observe the safety banner: *"Image text could not be verified with clinical certainty. Please take another photo in good lighting."*
4. **Live Multi-Agent Telemetry**:
   - Upload a clear packaging image (e.g. *Advil* or *Amoxicillin*).
   - Watch the agent status panel update in real-time with duration in milliseconds for each agent.
5. **Confirm Identity**:
   - Review the candidate matches with confidence scores.
   - Click **Confirm & View Medicine Info**.
6. **Verified Monograph with Official Source Links**:
   - Review the verified monograph sections with `Verified via FDA Label` badges.
   - View the official publication/revision date.
   - Click the **DailyMed (NLM)** link to open the official FDA drug label in a new tab.
