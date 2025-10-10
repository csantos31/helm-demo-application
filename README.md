# Helm Demo Application
![GitHub Actions](https://img.shields.io/badge/github%20actions-%232671E5.svg?style=for-the-badge&logo=githubactions&logoColor=white)
![Docker](https://img.shields.io/badge/docker-%230db7ed.svg?label=GHCR%20Image&style=for-the-badge&logo=docker&logoColor=white)
![React](https://img.shields.io/badge/react-%2320232a.svg?style=for-the-badge&logo=react&logoColor=%2361DAFB)

---

This repository contains a simple React application built from the default `create-react-app` template.  
Although the application itself has no functional modifications, this project focuses on demonstrating containerization and CI/CD automation for deployment workflows.

---

## 🧩 Project Overview

The purpose of this repository is to serve as a **base application** to generate a Docker image that will be used by a Helm chart in a separate repository ([helm-demo-chart](https://github.com/csantos31/helm-demo-chart)).

---

## 🛠️ Tech Stack

- **React** (via `create-react-app`)
- **Docker**
- **GitHub Actions**
- **GitHub Container Registry (GHCR)**
- **Nginx** (as the web server in the container)

---

## 🐳 Dockerization

The `Dockerfile` defines the build and runtime environment for the React app.  
It uses a **multi-stage build** approach to optimize image size:

1. **Build stage:** Compiles the React app using Node.js.
2. **Runtime stage:** Serves the static files using Nginx.

```Dockerfile
# Example structure
FROM node:18 AS build
WORKDIR /app
COPY . .
RUN npm ci && npm run build

FROM nginx:alpine
COPY --from=build /app/build /usr/share/nginx/html
```

## ⚙️ GitHub Actions (CI/CD)

The repository includes a GitHub Actions workflow that:

Builds the Docker image.

Pushes it to GitHub Container Registry (GHCR) under your account.

This ensures that any commit to the main branch triggers a rebuild and publishes the updated image automatically.

> Example workflow file: .github/workflows/docker-image.yml

## 🚀 Running Locally

To run the app locally without Docker:
```
npm install
npm start
```

To build and run via Docker:
```
docker build -t helm-demo-app .
docker run -p 3000:80 helm-demo-app
```

## 🏗️ Deployment Integration

This application is consumed by a Helm chart located at:
👉 helm-demo-chart

That chart pulls the image built and hosted via GHCR, enabling Kubernetes deployment.

## 📚 Learnings & Goals

* Implemented a full CI/CD pipeline using GitHub Actions and GHCR.

* Practiced containerization of a front-end React app with Nginx.

* Served as the foundation for a Helm-based deployment workflow.