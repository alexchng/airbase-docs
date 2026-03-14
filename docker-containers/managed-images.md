# Managed Images

💡 Reminder: SGTS GLCR images are prefixed with `registry.sgts.gitlab-dedicated.com/innersource/sgts/runtime/airbase/images`

Node · Python · Go · Nginx · Debian

#### Supported Versions

Node

| Tag             | Docker Hub Image                     |
| --------------- | ------------------------------------ |
| node-24         | gdssingapore/airbase:node-24         |
| node-24-builder | gdssingapore/airbase:node-24-builder |
| node-22         | gdssingapore/airbase:node-22         |
| node-22-builder | gdssingapore/airbase:node-22-builder |
| node-20         | gdssingapore/airbase:node-20         |
| node-20-builder | gdssingapore/airbase:node-20-builder |

Sample Usage:

{% code title="Dockerfile" %}
```dockerfile
FROM gdssingapore/airbase:node-22-builder AS builder
ENV NEXT_TELEMETRY_DISABLED=1
ENV SKIP_ENV_VALIDATION=1
COPY package.json package-lock.json ./
COPY prisma ./prisma
RUN npm install
COPY . ./
RUN npm run build

FROM gdssingapore/airbase:node-22-builder AS prisma
COPY prisma ./prisma
RUN npx prisma generate

FROM gdssingapore/airbase:node-22
RUN mkdir .next && chown app:app .next
RUN mkdir .npm && chown app:app .npm
COPY --from=builder --chown=app:app /app/.next/standalone ./
COPY --from=builder --chown=app:app /app/.next/static ./.next/static
COPY --from=builder --chown=app:app /app/public ./public
COPY --from=builder --chown=app:app /app/prisma ./prisma
COPY --from=prisma --chown=app:app /root/.npm/_npx ./.npm/_npx
USER app
CMD ["node", "server.js", "--port", "$PORT"]
```
{% endcode %}

Python

| Tag         | Docker Hub Image                 |
| ----------- | -------------------------------- |
| python-3.14 | gdssingapore/airbase:python-3.14 |
| python-3.13 | gdssingapore/airbase:python-3.13 |

Sample Usage:

{% code title="Dockerfile" %}
```dockerfile
FROM gdssingapore/airbase:python-3.14
ENV PYTHONUNBUFFERED=TRUE
COPY --chown=app:app requirements.txt ./
RUN pip install -r requirements.txt
COPY --chown=app:app . ./
USER app
CMD ["bash", "-c", "streamlit run main.py --server.port=$PORT"]
```
{% endcode %}

Go

| Tag                 | Docker Hub Image                         |
| ------------------- | ---------------------------------------- |
| golang-1.25-builder | gdssingapore/airbase:golang-1.25-builder |
| golang-1.24-builder | gdssingapore/airbase:golang-1.24-builder |
| golang-1.23-builder | gdssingapore/airbase:golang-1.23-builder |

Sample Usage:

{% code title="Dockerfile" %}
```dockerfile
FROM gdssingapore/airbase:golang-1.25-builder AS build_go
COPY go.* ./
RUN go mod download && go mod verify
COPY . .
RUN go build -o app

FROM gdssingapore/airbase:debian-13
USER app
COPY --from=build_go --chown=app:app /app/app ./
CMD ["./app"]
```
{% endcode %}

Nginx

| Tag        | Docker Hub Image                |
| ---------- | ------------------------------- |
| nginx-1.28 | gdssingapore/airbase:nginx-1.28 |

Sample Usage:

{% code title="Dockerfile" %}
```dockerfile
# Note: Our managed nginx image configures nginx to listen on port 3000, not 80!
# Put your static files to be hosted in /usr/share/nginx/html
FROM gdssingapore/airbase:nginx-1.28
COPY --chown=app:app index.html /usr/share/nginx/html/
```
{% endcode %}

Debian

| Tag       | Docker Hub Image               |
| --------- | ------------------------------ |
| debian-13 | gdssingapore/airbase:debian-13 |
| debian-12 | gdssingapore/airbase:debian-12 |
