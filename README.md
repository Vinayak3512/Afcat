# afcatPYQ — AFCAT 2026 Prediction & Dashboard

A data science pipeline that analyzes previous-year questions (PYQ) to predict high-priority topics, generate study plans, and publish an interactive dashboard for AFCAT 2026 preparation.

> ⚠️ **Disclaimer:** This system predicts topic patterns and distributions (60–75% accuracy), not exact questions (25–40% accuracy at best). Use it to optimize preparation, not as a crystal ball.

---

## Features

- **Topic Frequency Analysis** — Identifies high-probability topics from historical data
- **Trend Detection** — Tracks rising and falling topic importance across years
- **Difficulty Prediction** — Classifies expected difficulty levels per topic
- **Current Affairs Scoring** — Rates news articles by AFCAT relevance
- **Study Plan Generator** — Produces an optimized 20-day study plan and mock-test blueprint
- **AI Practice Questions** — Generates practice questions based on predicted patterns
- **Static Dashboard** — Publishes an HTML + JSON dashboard under `output/predictions_2026/`

---

## Project Structure

```
afcatPYQ/
├── data/
│   ├── raw/                  # Raw question papers (JSON/CSV)
│   ├── processed/            # Cleaned and labeled data
│   └── sample/               # Sample data for testing
├── models/
│   ├── topic_predictor.py    # XGBoost topic frequency model
│   ├── difficulty_model.py   # Random Forest difficulty classifier
│   └── current_affairs.py   # News relevance classifier
├── utils/
│   ├── data_loader.py        # Data loading and preprocessing
│   ├── feature_engine.py     # Feature engineering
│   └── bias_correction.py   # Memory recall bias correction
├── analysis/
│   ├── topic_analyzer.py     # Historical topic analysis
│   └── trend_detector.py    # Temporal trend detection
├── scripts/
│   ├── generate_complete_data.py  # Dashboard bundle generator
│   └── mark_certain_topics.py    # Topic confidence marker
├── output/
│   └── predictions_2026/    # Generated dashboard (data.js, JSONs, dashboard.html)
├── .github/workflows/
│   └── deploy-dashboard.yml # CI/CD for GitHub Pages
├── main.py                  # Main entry point
├── config.py                # Configuration settings
└── requirements.txt         # Dependencies
```

---

## Quick Start

```bash
# 1. Install dependencies
pip install -r requirements.txt

# 2. Run the prediction system
python main.py

# 3. Generate a study plan
python main.py --generate-plan

# 4. Analyze current affairs
python main.py --analyze-news

# 5. Regenerate the dashboard bundle
python scripts/generate_complete_data.py

# 6. Preview the dashboard locally
python -m http.server 8000 --directory output/predictions_2026
# Open http://localhost:8000/dashboard.html
```

---

## Deployment

### Vercel (Recommended)

1. Rename the dashboard entry point before pushing:
   ```bash
   mv output/predictions_2026/dashboard.html output/predictions_2026/index.html
   git add output/predictions_2026/index.html
   git commit -m "chore: rename dashboard to index.html for Vercel"
   git push
   ```
2. In Vercel, import your GitHub repo and set:
   - **Framework Preset:** Other
   - **Build Command:** *(leave empty, or `python scripts/generate_complete_data.py` to build on deploy)*
   - **Output Directory:** `output/predictions_2026`
3. Deploy. Vercel will serve `index.html` alongside JSON files, so `fetch('afcat_2026_predictions.json')` works without path changes.

### GitHub Pages

Use the included `.github/workflows/deploy-dashboard.yml` — it regenerates the dashboard and publishes to GitHub Pages automatically. You can also use `gh-pages` branch or the `actions-gh-pages` action.

### Netlify

Set the build command to `python scripts/generate_complete_data.py` and the publish directory to `output/predictions_2026`.

---

## Expected Accuracy

| Prediction Type            | Accuracy      | Best Used For              |
|---------------------------|---------------|----------------------------|
| Topic Distribution         | 65–75%        | Knowing which topics appear |
| Question Count per Topic   | ±1–2 questions| Time allocation             |
| Difficulty Level           | 55–60%        | Preparation intensity       |
| Specific Questions         | 25–40%        | Identifying similar patterns|

---

## Key Insights for AFCAT 2026

### High-Priority Topics (80%+ Confidence)

| Subject   | Topics                                          |
|-----------|-------------------------------------------------|
| Math      | Speed/Distance, Time/Work, Profit/Loss          |
| English   | Cloze Test, OLQ Vocabulary (Synonyms/Antonyms)  |
| Reasoning | Dot Situation, Spatial Ability, Venn Diagrams   |
| GK        | Defence Operations, Sports Terms, Modern History|

### Structural Changes for 2026
- Math section increased from 18 → 20 questions
- EKT discontinued (may shift weight to GK science questions)
- Greater focus on Officer-Like Qualities (OLQ) vocabulary

---

## Configuration

Edit `config.py` to customize:
- Historical data years to include
- Confidence thresholds
- Topic categories
- Model hyperparameters

---

## Development Notes

- `pyqData` is embedded in `data.js` during bundle generation. The dashboard script copies top-level consts into `window.*` so the UI works when `data.js` defines `const pyqData = ...` rather than `window.pyqData`.
- To debug topic-to-PYQ links, open the browser console and inspect `window.pyqData` and `window.predictionsData`.
- Large PDF scans and data files may exist under `data/`. Consider removing binary artifacts from the repo and storing them externally if size becomes an issue.

---

## License

For educational purposes only. Use legally acquired data sources.

---

## Contributing / Support

Open an issue on GitHub for help with automated deploys to Vercel, Netlify, or GitHub Pages.
