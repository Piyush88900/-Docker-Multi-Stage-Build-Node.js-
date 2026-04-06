# 🚀 Docker Multi-Stage Build (Node.js Application)

This project demonstrates how to use **Docker multi-stage builds** to create an optimized, production-ready container image for a Node.js application.

---

## 📌 Overview

Multi-stage builds allow us to:

* Separate build and runtime environments
* Reduce final image size
* Improve security by excluding unnecessary dependencies

This project uses **3 stages**:

---

## 🏗️ Architecture

![Docker Multi-Stage Build](images/docker-multistage.png)

---

## ⚙️ Stages Explained

### 🔹 Stage 1: Dependencies (deps)

* Base image: `node:20-alpine`
* Installs all dependencies (including dev dependencies)
* Uses `npm ci` for clean installation

```dockerfile
FROM node:20-alpine AS deps
WORKDIR /app
COPY package*.json ./
RUN npm ci
```

---

### 🔹 Stage 2: Builder

* Uses dependencies from Stage 1
* Copies source code
* Builds the application

```dockerfile
FROM deps AS builder
COPY . .
RUN npm run build
```

---

### 🔹 Stage 3: Production

* Lightweight final image
* Copies only required build output
* Installs only production dependencies

```dockerfile
FROM node:20-alpine AS production
WORKDIR /app
COPY --from=builder /app/dist ./dist
COPY package*.json ./
RUN npm ci --omit=dev
CMD ["node", "dist/index.js"]
```

---

## ✅ Benefits

* 🚀 Smaller image size
* 🔒 Improved security (no dev dependencies in production)
* ⚡ Faster deployments
* 🧹 Clean and efficient build process

---

## 🛠️ How to Run

### 1️⃣ Build the Docker Image

```bash
docker build -t my-node-app .
```

### 2️⃣ Run the Container

```bash
docker run -p 3000:3000 my-node-app
```

---

## 📂 Project Structure

```
project/
├── Dockerfile
├── package.json
├── src/
│   └── index.js
├── k8s/                 
├── images/
│   └── docker-multistage.png
├── .dockerignore
├── .gitignore
└── README.md
```

---

## 💡 Key Learning

This project demonstrates:

* Docker multi-stage builds
* Production-ready containerization
* Efficient dependency management
* Clean DevOps practices

---

## 🚀 Future Improvements

* Add CI/CD pipeline (GitHub Actions / Jenkins)
* Deploy to Kubernetes / EKS
* Add monitoring and logging

---

## 🙌 Author

**Piyush**
Aspiring DevOps Engineer 🚀

---
