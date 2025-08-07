# Devopstask1
Elevate Labs Tasks
# 🚀 Node.js CI/CD with GitHub Actions & DockerHub

This project demonstrates how to set up a CI/CD pipeline using **GitHub Actions** to:
- ✅ Build a Node.js app
- ✅ Run tests
- ✅ Build a Docker image
- ✅ Push the image to DockerHub automatically on every `git push` to `main`

---

## 📦 Project Structure

.
├── app.js # Simple Node.js server
├── Dockerfile # Docker configuration
├── package.json # Node.js dependencies
└── .github
└── workflows
└── main.yml # GitHub Actions pipeline

---

## 🛠️ Setup Instructions

### 🔐 1. Get Your DockerHub Username

If you signed in via **Google**, follow these steps:

- Go to [https://hub.docker.com/account](https://hub.docker.com/account)
- Copy your **DockerHub username**

### 🛡 2. Create DockerHub Access Token

> You can’t use Google login password for CI/CD. You need a token.

1. Go to [DockerHub Security Settings](https://hub.docker.com/settings/security)
2. Click **"New Access Token"**
3. Name it (e.g., `github-actions`)
4. Click **Create** and **copy the token** (you won’t be able to see it again)

---

### 🔑 3. Add Secrets in GitHub Repo

1. Go to your GitHub repository  
2. Navigate to: **Settings → Secrets and variables → Actions**
3. Click **New repository secret** and add:

| Name               | Value                                  |
|--------------------|----------------------------------------|
| `DOCKER_USERNAME`  | Your DockerHub username                |
| `DOCKER_PASSWORD`  | Your DockerHub access token (step 2)   |

---

### 🚀 4. Push Code to Main

Make sure your code is on the `main` branch and push:
```bash
git push origin main
GitHub Actions will:

🧪 Run tests

🐳 Build the Docker image

☁️ Push it to DockerHub as:
docker.io/<your-username>/my-node-app:latest

🐳 Dockerfile
FROM node:16-alpine

WORKDIR /app

COPY package*.json ./
RUN npm install

COPY . .

EXPOSE 3000

CMD ["node", "app.js"]

📜 GitHub Actions Workflow (.github/workflows/main.yml)
name: CI/CD Pipeline

on:
  push:
    branches:
      - main

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout Code
        uses: actions/checkout@v3

      - name: Set up Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '16'

      - name: Install Dependencies
        run: npm install

      - name: Run Tests
        run: npm test

      - name: Log in to DockerHub
        run: echo "${{ secrets.DOCKER_PASSWORD }}" | docker login -u "${{ secrets.DOCKER_USERNAME }}" --password-stdin

      - name: Build Docker Image
        run: docker build -t ${{ secrets.DOCKER_USERNAME }}/my-node-app .

      - name: Push Docker Image
        run: docker push ${{ secrets.DOCKER_USERNAME }}/my-node-app

✅ Test Locally (Optional)
docker build -t your-username/my-node-app .
docker run -p 3000:3000 your-username/my-node-app

🧠 What’s Next?
🟢 Deploy this image to AWS ECS Fargate

🔁 Add image versioning using GITHUB_SHA or Git tags

📦 Push to AWS ECR instead of DockerHub

🧑‍💻 Author
Built with ❤️ by meher Prudhvi


---

Let me know if you want:
- A version that includes AWS ECS deployment
- An Express.js version of the app
- Automatic tag versioning in the workflow
