FROM python:3.10 AS build

ENV PYTHONUNBUFFERED 1
ENV PYTHONDONTWRITEBYTECODE 1
ENV SECRET_KEY "build-time-dummy-secret-key-not-used-in-production"
ENV DJANGO_SETTINGS_MODULE "application.settings"

ARG APPLICATION_NAME=application

LABEL maintaner="andreichiro@gmail.com"
LABEL .br.com.andreichiro-schema.application=$APPLICATION_NAME

WORKDIR /app
RUN apt-get update && apt-get install -y --no-install-recommends postgresql-client git && rm -rf /var/lib/apt/lists/*
COPY . .
RUN pip3 install --upgrade pip
RUN pip3 install -r requirements.txt --no-cache-dir

RUN python manage.py collectstatic --noinput --clear 2>/dev/null || echo "collectstatic warning - continuing"

EXPOSE 8000

ENTRYPOINT gunicorn application.wsgi:application --bind 0.0.0.0:8000 --workers 2 --timeout 120
