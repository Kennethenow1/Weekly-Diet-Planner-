# Weekly Diet Planner

A full-stack application that collects pantry items and wellness goals, preprocesses them, calls an OpenAI model to craft weekday meal plans, stores the results, and renders them in a polished dashboard and React interface.

## Key Features
- Guided input flow with available ingredients, weekly goal, and dietary preference (prebuilt diets + custom option).
- Pipeline automatically preprocesses inputs, generates a plan with OpenAI, validates and cleans the output, and renders an HTML dashboard.
- FastAPI backend exposes REST endpoints for plan generation, CSV download, dashboard output, and plan history.
- React front end mirrors the look-and-feel of the dashboard, provides an interactive form, and shows saved plans.
- SQLite storage keeps both the current dashboard view and a history of generated plans.

## Requirements
- Python 3.12 (system python) with ability to install packages using `pip --break-system-packages` (already bootstrapped in this repo).
- Node.js LTS (18+ recommended) for the React development server.
- An OpenAI API key with access to the configured model.

## Project Structure
```
Food_generation_Apllication/
├── app/                 # FastAPI server and config loader
├── pipeline/            # Pipeline stages: input collection → preprocessing → ai_model → postprocessing → storage → rendering
├── frontend/            # Vite + React client with form UI and history viewer
├── data/                # Generated CSV outputs (preprocessing + model outputs)
├── pipeline/rendering/  # HTML/CSS templates and index.db storage
├── README.md            # This file
└── requirements.txt     # Python dependencies
```

## Backend Setup
1. **Install dependencies (already installed if you followed previous steps):**
   ```bash
   ~/.local/bin/pip install --break-system-packages -r requirements.txt
   ```
2. **Create a `.env` file at the repo root:**
   ```bash
   cp .env.example .env   # if you add an example file
   ```
   Then open `.env` and set at least:
   ```
   OPENAI_API_KEY=your_api_key
   MODEL_NAME=gpt-4
   ```
   (You can override `FRONTEND_ORIGIN`, `AI_OUTPUT_DIR`, etc. as needed.)
3. **Run the API server:**
   ```bash
   PYTHONPATH=. python3 -m uvicorn app.server:app --host 0.0.0.0 --port 8000
   ```

## Frontend Setup
1. Install Node dependencies:
   ```bash
   cd frontend
   npm install
   ```
2. Start the development server:
   ```bash
   npm run dev
   ```
3. Visit `http://localhost:5173` to use the React UI. It communicates with the FastAPI backend at `http://localhost:8000` (configure via `VITE_API_BASE` if different).

## Workflow Overview
1. User enters pantry items, weekly goal, and diet preference in the React form.
2. Frontend calls `POST /plan`; FastAPI runs the pipeline:
   - Preprocesses inputs → generates an OpenAI prompt → calls the model with the configured `MODEL_NAME`.
   - Validates/cleans the JSON response → persists CSV, dashboard HTML, and plan history in SQLite.
3. API returns the plan while the dashboard can be opened at `/dashboard`; CSV at `/plan/csv`.
4. History of past plans is available via `GET /history` and surfaced in the React UI.

## Tips
- **Never commit `.env`**: add it to `.gitignore` so secrets stay local.
- Plan history lives in `pipeline/rendering/index.db`; remove or back it up as needed.
- If the OpenAI project lacks access to the configured model, update `MODEL_NAME` or request access.
- The backend already handles sanitizing model responses, but prompt tweaks can improve consistency.

## Scripts
From repo root:
- `PYTHONPATH=. python3 -m uvicorn app.server:app --reload` — run backend in dev with auto-reload.
- `npm --prefix frontend run build` — build production bundle (output in `frontend/dist`).

## License
Add your preferred license text here.
