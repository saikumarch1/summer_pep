# Project Overview

`summer_pep_project` is a lightweight Flask web application designed to demonstrate an automated end-to-end DevOps CI/CD pipeline using GitHub Actions, DockerHub, and Render.

## Features

- **Home Page (`/`)**: Student portal interface with schedule and assignment modules.
- **About Page (`/about`)**: Overview of the project and CI/CD workflow.
- **Health Check Endpoint (`/health`)**: API endpoint returning application health status in JSON.

## Technologies Used

- **Framework**: Flask (Python)
- **Frontend**: HTML5, CSS3
- **Containerization**: Docker
- **Automation / CI/CD**: GitHub Actions
- **Container Registry**: DockerHub
- **Cloud Hosting**: Render

## Folder Structure

```text
summer_pep_project/
│
├── app.py
├── requirements.txt
├── Dockerfile
├── .dockerignore
├── README.md
├── templates/
│     ├── index.html
│     └── about.html
├── static/
│     └── style.css
└── .github/
      └── workflows/
            ci-cd.yml
```

## Running Locally

1. Install dependencies:
   ```bash
   pip install -r requirements.txt
   ```

2. Run the application:
   ```bash
   python app.py
   ```

3. Open your browser and navigate to `http://localhost:5000`.

## Docker Commands

1. Build the Docker image:
   ```bash
   docker build -t summer_pep_project .
   ```

2. Run the Docker container:
   ```bash
   docker run -p 5000:5000 summer_pep_project
   ```

3. Access the containerized app at `http://localhost:5000`.

## GitHub Actions Workflow

The pipeline triggers on every push or pull request to `main`/`master` branches and consists of three jobs:

1. **Test**: Installs Python dependencies and runs application verification.
2. **Build & Push**: Authenticates with DockerHub, builds the Docker image, and pushes `<username>/summer_pep_project:latest`.
3. **Deploy**: Triggers Render deployment via Deploy Hook URL.

### Required GitHub Secrets

Configure the following secrets in GitHub Repository Settings -> Secrets and variables -> Actions:

- `DOCKERHUB_USERNAME`: Your DockerHub username.
- `DOCKERHUB_TOKEN`: Your DockerHub Personal Access Token (PAT).
- `RENDER_DEPLOY_HOOK_URL`: Your Render Web Service Deploy Hook URL.

## DockerHub

1. Create a public repository named `summer_pep_project` on [DockerHub](https://hub.docker.com/).
2. Push images manually (optional):
   ```bash
   docker tag summer_pep_project <your-dockerhub-username>/summer_pep_project:latest
   docker push <your-dockerhub-username>/summer_pep_project:latest
   ```

## Render Deployment

1. Create a new **Web Service** on [Render](https://render.com/).
2. Select **Deploy an existing image from a registry**.
3. Provide your DockerHub image URL: `docker.io/<your-dockerhub-username>/summer_pep_project:latest`.
4. Set container port to `5000`.
5. Copy the **Deploy Hook** URL from Render settings and add it as `RENDER_DEPLOY_HOOK_URL` in GitHub Secrets.

## CI/CD Architecture

```text
Local Project
      │
git add
git commit
git push
      │
      ▼
GitHub Repository
      │
GitHub Actions
      │
 ┌──────────────┐
 │ Test         │
 └──────────────┘
        │
        ▼
 ┌────────────────────────┐
 │ Build Docker Image     │
 └────────────────────────┘
        │
        ▼
 Push Image to DockerHub
        │
        ▼
Render pulls latest Docker image
        │
        ▼
Application Live
```
