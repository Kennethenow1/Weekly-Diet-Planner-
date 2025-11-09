# Main Aim Of Project
The main aim for this project was to develop something that I can use myself, something that will aid me in developing a healthy diet. What is that something? Well, an AI algorithm. Because I am too lazy and don’t know the specifics of every meal, ending up with a pizza in my mouth every time I tell myself I’ll eat healthier, I want to develop something that can use an educated decision tree to tell me what I should eat to achieve my goals. This educated decision tree will be based off the IU Dining Eats website.

## Version 2 (Dataset of IU Eats website was intergrated. Not yet pushed will do 9th 11:59 est)

- 1st(page); The Kitchen—choose dataset(added the IU Dining Eats data) or pantry mode, set dates, and define your goal.
- 2nd(page); The Dish—a modern black-and-white layout showing daily breakfast/lunch/dinner tiles with detailed overlays.
- 3rd(page); The Pantry—view, load, or delete prior meal plans.
- Smarter nutrition: Every meal now carries a full nutrient panel (Calories, macros, key vitamins/minerals, etc.), rolled up into a daily total.
- Menu variety safeguards: Backend enforces unique breakfasts, lunches, and dinners across the time window, automatically nudging the model when it tries to repeat itself.
- Housekeeping: Added a history delete endpoint and improved CLI/JSON handling so big menu datasets no longer break preprocessing.
<img width="1857" height="902" alt="Screenshot from 2025-11-09 01-09-29" src="https://github.com/user-attachments/assets/293ac9bc-b429-4f52-81c7-7d42608d83a7" />
<img width="1825" height="902" alt="Screenshot from 2025-11-09 01-10-34" src="https://github.com/user-attachments/assets/7624db15-f8b4-4929-b5f3-d68dd7fe181f" />
<img width="1834" height="902" alt="Screenshot from 2025-11-09 01-10-57" src="https://github.com/user-attachments/assets/9748285d-5a80-463b-98bf-49ba82c39ed7" />
<p align="center">
  <img width="530" height="850" alt="Screenshot from 2025-11-09 01-11-28" src="https://github.com/user-attachments/assets/f16bc0bd-eb54-40ba-bd51-ae9c8f2413ab" />
</p>
<img width="1000" height="902" alt="Screenshot from 2025-11-09 01-11-47" src="https://github.com/user-attachments/assets/a94fa418-58fc-461b-8b24-0aa8f5a36ce1" />


## Version 1 (Working pipeline to get information to the API and for it to respond like wanted). Weekly Diet Planner
A full-stack application that collects pantry items and wellness goals, preprocesses them, calls an OpenAI model to craft weekday meal plans, stores the results, and renders them in a polished dashboard and React interface.

#### Key Features
- Guided input flow with available ingredients, weekly goal, and dietary preference (prebuilt diets + custom option).
- Pipeline automatically preprocesses inputs, generates a plan with OpenAI, validates and cleans the output, and renders an HTML dashboard.
- FastAPI backend exposes REST endpoints for plan generation, CSV download, dashboard output, and plan history.
- React front end mirrors the look-and-feel of the dashboard, provides an interactive form, and shows saved plans.
- SQLite storage keeps both the current dashboard view and a history of generated plans.

<img width="877" height="902" alt="Screenshot from 2025-11-08 11-13-45" src="https://github.com/user-attachments/assets/dfae971f-499b-43bf-ba65-9d3090928bbd" />
<img width="877" height="902" alt="Screenshot from Screencast from 2025-11-08 11-15-13 webm" src="https://github.com/user-attachments/assets/7008763d-5da2-44a4-bcd6-ab42b4c9f63f" />
<img width="877" height="902" alt="Screenshot from 2025-11-08 11-14-07" src="https://github.com/user-attachments/assets/d18240f7-ca46-488a-a21d-ed6865270388" />
<img width="877" height="902" alt="Screenshot from 2025-11-08 11-16-40" src="https://github.com/user-attachments/assets/eeb1c559-2997-4c64-9b93-c846529dacba" />


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
MIT License

Copyright (c) 2025 Kennethenow1

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
#Version 2 ( will work specifically with the iu eats database as reference for sudgestion mainly beacause i am too lazy and eating too unhealthy and need some structure in my life)
