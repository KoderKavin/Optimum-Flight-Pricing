# Predict-It — Streamlit inference UI
# Build from project root (not this folder):
#   docker build -f build_app/Dockerfile -t predict-it-ui:latest .

FROM python:3.11-slim-bookworm

WORKDIR /app

ENV PYTHONDONTWRITEBYTECODE=1 \
    PYTHONUNBUFFERED=1 \
    STREAMLIT_SERVER_HEADLESS=true \
    STREAMLIT_BROWSER_GATHER_USAGE_STATS=false

COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# app.py resolves paths relative to project root (parent of build_app)
COPY dataset/ ./dataset/
COPY starter_kit/ ./starter_kit/
COPY build_app/app.py ./build_app/

# Generate dataset and train model during build since model.joblib is ignored by Git
RUN python starter_kit/train.py

WORKDIR /app/build_app

EXPOSE 8501

CMD ["streamlit", "run", "app.py", "--server.address=0.0.0.0", "--server.port=8501"]
