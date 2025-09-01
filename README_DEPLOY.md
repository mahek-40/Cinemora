# Quick Deploy Guide

You have two working entry points:
- `portal.py` (uses TMDB API to fetch posters; replace the hardcoded API key with your own)
- `portal_offline.py` (no network needed; uses placeholder posters)

## Streamlit Community Cloud (Recommended)
1. Push this folder to a **public GitHub repo**.
2. On https://share.streamlit.io, click **New app** and point to your repo.
3. Set **Main file** to `portal.py` (or `portal_offline.py` if your TMDB key isn't ready).
4. Add the following **secrets** (optional but recommended if you use `portal.py`):
   ```
   TMDB_API_KEY = "your_tmdb_key_here"
   ```
5. Deploy.

## Render.com (works like Heroku)
1. Push to GitHub.
2. Create a new **Web Service**.
3. **Build Command**: `pip install -r requirements.txt`
4. **Start Command**: `sh setup.sh && streamlit run portal.py --server.port=$PORT --server.address=0.0.0.0`
5. Select Python version 3.10 (or use `runtime.txt`).

## Heroku (if you still use it)
- `Procfile`, `runtime.txt`, and `setup.sh` are ready.
- `heroku buildpacks:add heroku/python`
- `git push heroku main`

## Common Fixes
- **Huge model file**: `artifacts/similarity.pkl` is ~185 MB. Many platforms allow it, but the first boot can be slow.
- **Missing dependencies**: `requirements.txt` now includes `pandas`, `numpy`, and `scikit-learn`—these are often needed for pickled artifacts.
- **API Key**: The TMDB key in `portal.py` is a public demo key and may fail. Put your own in Streamlit **secrets** and read it with:
  ```python
  import os
  TMDB_API_KEY = os.getenv("TMDB_API_KEY", "")
  ```
- **Paths**: Code reads from `artifacts/movie_list.pkl` and `artifacts/similarity.pkl`. Keep the working directory at the project root.
