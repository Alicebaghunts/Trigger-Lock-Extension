# Trigger Lock

Trigger Lock is a Chrome extension (Manifest V3) that helps users spot phishing / malicious social‑engineering text while browsing.

## What it does

- On‑page checking: select text on any site and click the Trigger Lock marker — result shown as an on‑page modal/banner UI.
- Popup checking: paste text into the extension popup (`frontend/index.html`) and submit for analysis.
- Context menu: right‑click selected text → "Anti Phishing check" (created by the background service worker).

## Architecture

- Frontend (extension) — `frontend/`
  - `manifest.json` — permissions, content scripts, background service worker
  - `scripts/content.js` — injects UI, captures selected text, triggers analysis
  - `background.js` — creates context menu and forwards checks to API
  - `index.html` — popup UI (paste text and send for analysis)

- Backend (ML API) — `server/`
  - FastAPI exposes `POST /predict`
  - Loads TF‑IDF vectorizer + SVM model from `server/saved_data/`
  - Returns `{ "prediction": 0 | 1 }` (0 = safe, 1 = phishing)

## Quick start

1) Start the backend API

- create and activate virtualenv (zsh)
  - cd server
  - python3 -m venv .venv
  - source .venv/bin/activate
- install dependencies
  - pip install -r requirements.txt
- run the API (common options):
  - If `server.py` exposes a FastAPI `app` object:

    uvicorn server:app --reload --host 127.0.0.1 --port 8000

  - Or run directly (fallback):

    python3 server.py

- Verify with curl:

  curl -s -X POST http://127.0.0.1:8000/predict -H "Content-Type: application/json" -d '{"text":"sample text"}'

  Expected: `{ "prediction": 0 }` or `{ "prediction": 1 }`

2) Load the extension (Chrome / Edge)

- Open `chrome://extensions`
- Enable Developer mode
- Click “Load unpacked” and select the `frontend/` folder
- Open the popup or navigate to a page, select text and use the marker/context menu to trigger checks
- Inspect extension background logs via the service worker devtools for debugging

3) Quick static preview for popup

- From the repository root:
  - cd frontend
  - python3 -m http.server 8000
  - Open `http://localhost:8000` and open `index.html` to view the popup UI

> Note: to fully test background/content behavior load the unpacked extension in the browser as described above.

## API contract

- Endpoint: `POST /predict`
- Request body: `{ "text": "<string to analyze>" }`
- Response: `{ "prediction": 0 | 1 }`

## File layout (high level)

```
frontend/
  src/                # development source (JS, components, background, content)
  scripts/            # current source used by the extension (popup/background/content)
  css/
  images/
  manifest.json
server/
  server.py
  requirements.txt
  saved_data/         # trained model artifacts (svm_model.pkl, tfidf_vectorizer.pkl)
```

## Notes & recommendations

- Model files in `server/saved_data/` are binary artifacts — consider Git LFS or external storage for large models.
- Migrate frontend source into `frontend/src/` and add a bundler (Vite or webpack) for developer ergonomics and builds.
- Add linting (`.eslintrc.json`) and basic CI to run server tests and frontend linting.

## Contributing

- Open issues for bugs or feature requests.
- Pull requests should include a description, testing steps, and be targeted at `main`.

## License

Add a `LICENSE` file (e.g. MIT) if you plan to open source this repository.

