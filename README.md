# Crop Disease Detection (Modernized)

A Flask + TensorFlow web app for crop leaf disease prediction using a trained `.hdf5` model.

## What was upgraded

- Modern Python dependency stack (Flask 3.x, TensorFlow CPU 2.15, NumPy 1.26, Pillow 11)
- Refactored backend to use `tensorflow.keras` and safer request/error handling
- Improved prediction preprocessing with Pillow (`RGB` convert + resize)
- Better frontend UX (modern layout, loading state handling, validation, responsive styles)
- Fixed static asset path issues on Linux (case-sensitive filenames)

## Tech Stack

- Python 3.11+
- Flask 3.1.1
- TensorFlow CPU 2.15.1
- NumPy 1.26.4
- Pillow 11.2.1
- Gunicorn 23.0.0
- Bootstrap 5.3 + jQuery 3.7

## Project Structure

```text
app.py
requirements.txt
Procfile
templates/
  base.html
  index.html
static/
  css/main.css
  js/main.js
  Screenshot_2.png
  Screenshot_3.png
  strawberry-plant-leaf-spot.jpg
uploads/
Model.hdf5
```

## Local Setup

```bash
cd /home/naman/Crop-Disease-Detection
/home/naman/.pyenv/versions/3.11.9/bin/python -m pip install -r requirements.txt
/home/naman/.pyenv/versions/3.11.9/bin/python app.py
```

Open: `http://127.0.0.1:5000`

## How Prediction Works

1. User uploads a leaf image (`.jpg/.jpeg/.png`)
2. Backend saves file to `uploads/`
3. Image is preprocessed to `224x224`, normalized, and passed to model
4. Model returns class index; app maps index to crop + disease label
5. Prediction is returned to UI and displayed

## API

### `POST /predict`

- Form-data field: `file`
- Success response: plain text (predicted crop and disease)
- Error response: JSON with `error` message and HTTP status code

## Run in Production

```bash
gunicorn app:app --bind 0.0.0.0:5000
```

`Procfile` already contains:

```text
web: gunicorn app:app --bind 0.0.0.0:$PORT
```

## Notes

- CPU-only TensorFlow is used by default.
- GPU warnings can be ignored on systems without CUDA.
- If port `5000` is busy:

```bash
lsof -i :5000 -t | xargs -r kill -9
```

## Current Model Scope

The model predicts only on the classes it was originally trained on.
Supported crop/disease overview images are shown in the app UI.

## Ownership

This project can be independently owned by pushing it to your own repository (without forking), which you already configured.
