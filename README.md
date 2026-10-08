# AI Chat

Минимальный web-чат для развёртывания через Docker Compose.

## Архитектура

Browser → FastAPI → OpenAI-compatible API

API-ключ никогда не передаётся в JavaScript и не хранится в Git.

## 1. Подготовить сервер

~~~bash
git clone https://github.com/uplink0/KEY.git
cd KEY
cp .env.example .env
~~~

## 2. Вставить API-ключ

Откройте .env:

~~~bash
nano .env
~~~

Найдите строку:

~~~env
LLM_API_KEY=PASTE_YOUR_LLM_API_KEY_HERE
~~~

и замените значение на ключ реального LLM-провайдера.

Также при необходимости измените:

~~~env
LLM_BASE_URL=https://api.openai.com/v1
LLM_MODEL=gpt-4.1-mini
~~~

Для другого OpenAI-compatible сервиса укажите его base URL и имя модели. Для локальной модели на самом сервере можно использовать, например, http://host.docker.internal:1234/v1.

## 3. Запустить

~~~bash
docker compose up -d --build
~~~

Проверка:

~~~bash
curl http://127.0.0.1:8080/health
~~~

Ожидаемый ответ:

~~~json
{"status":"ok"}
~~~

После этого откройте:

http://SERVER_IP:8080

## ВАЖНО ПРО credential-lab

Ключ вида lab_... из нашего локального credential-lab является ключом лабораторного API. Он НЕ является ключом OpenAI или другого LLM-провайдера и сам по себе не даст этому приложению доступ к модели.

Если вы хотите подключить локальный LLM, который предоставляет OpenAI-compatible API, укажите адрес этого сервиса в LLM_BASE_URL, а его собственный API key — в LLM_API_KEY.

## Production

Для внешнего доступа лучше поставить перед контейнером HTTPS reverse proxy (Nginx/Traefik/Caddy) и не публиковать порт 8080 напрямую в интернет.
