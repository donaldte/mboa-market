# À FAIRE (étape 3, P2-03) : l'image de l'API, en deux étages, comme au TP 7 et au TP 8.
#   Étage 1 : FROM python:3.12 AS construction ; un venv dans /opt/venv ; pip install -r requirements.txt
#   Étage 2 : FROM python:3.12-slim ; un compte mboa (uid 10001) ; le venv copié de l'étage 1 ; app.py ;
#             USER mboa ; EXPOSE 8000 ; un HEALTHCHECK toutes les 10 s sur http://127.0.0.1:8000/sante ;
#             CMD ["gunicorn", "--bind", "0.0.0.0:8000", "--workers", "2", "--no-control-socket", "app:app"]
# Ton modèle : ~/tp08/boutique/api/Dockerfile. Le support donne le fichier complet.

FROM python:3.12 AS construction 
RUN python3 -m venv /opt/venv 
ENV PATH=/opt/venv/bin:$PATH 
COPY requirements.txt . 
RUN pip install --no-cache-dir -r requirements.txt

FROM python:3.12-slim 
RUN groupadd --system --gid 10001 mboa && useradd --system --uid 10001 --gid mboa --no-create-home --shell /usr/sbin/nologin mboa 
COPY --from=construction /opt/venv /opt/venv 
WORKDIR /app 
COPY app.py . 
ENV PATH=/opt/venv/bin:$PATH PYTHONUNBUFFERED=1 
HEALTHCHECK --interval=10s --timeout=3s --start-period=10s --retries=3 \
    CMD ["python3", "-c", \
         "import urllib.request as u; u.urlopen('http://127.0.0.1:8000/sante', timeout=2)"]
CMD ["gunicorn", "--bind", "0.0.0.0:8000", "--workers", "2", "--no-control-socket", "app:app"]
