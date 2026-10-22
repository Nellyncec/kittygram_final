FROM python:3.9

WORKDIR /app

COPY requirements.txt .
RUN pip install -r requirements.txt --no-cache-dir

COPY . .

CMD ["sh", "-c", "python manage.py migrate && python manage.py collectstatic --noinput && cp -r /app/collected_static/. /backend_static/static/ && gunicorn --bind 0.0.0.0:8000 kittygram_backend.wsgi"]
