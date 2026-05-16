# ChessMind — Complete Deployment Guide

> Production-ready deployment guide for **ChessMind**, an AI-powered chess platform.

---

# ♟️ ChessMind

AI-powered Chess Platform with:

- Real-time gameplay
- AI opponent support
- Multiplayer support
- JWT Authentication
- WebSocket communication
- Dockerized infrastructure
- Ollama LLM integration

---

# 📦 Tech Stack

| Layer | Technology |
|---|---|
| Frontend | Next.js 14 |
| Backend | Spring Boot 3 |
| Language | Java 17 |
| Database | MySQL 8 |
| Cache | Redis 7 |
| AI Engine | Ollama + llama3 |
| Containerization | Docker |
| Orchestration | Docker Compose |

---

# 🚀 Features

- ♟️ Play chess against AI
- 🌐 Online multiplayer support
- 🔐 JWT-based authentication
- ⚡ Real-time WebSocket gameplay
- 🧠 Ollama AI integration
- 🐳 Fully Dockerized deployment
- 📦 Production-ready architecture
- ☁️ AWS EC2 compatible

---

# 📁 Project Structure

```bash
Chess/
│
├── backend/
├── frontend/
├── docker-compose.yml
├── .env.example
└── Readme.md
```

---

# 🖥️ System Requirements

| Requirement | Minimum |
|---|---|
| OS | Ubuntu 22.04 / 24.04 |
| RAM | 8 GB |
| CPU | 2 vCPU |
| Disk | 20 GB |

---

# 🔓 Required Open Ports

| Port | Purpose |
|---|---|
| 22 | SSH |
| 3000 | Frontend |
| 8080 | Backend API |
| 11434 | Ollama API |

---

# ⚙️ Step 1 — Update Server

```bash
sudo apt update && sudo apt upgrade -y
```

---

# 🐳 Step 2 — Install Docker

```bash
curl -fsSL https://get.docker.com | sudo sh
sudo usermod -aG docker $USER
newgrp docker
```

Verify:

```bash
docker --version
```

---

# 🧩 Step 3 — Install Docker Compose

```bash
sudo curl -L "https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname -s)-$(uname -m)" \
-o /usr/local/bin/docker-compose

sudo chmod +x /usr/local/bin/docker-compose
```

Verify:

```bash
docker compose version
```

---

# 📥 Step 4 — Clone Repository

```bash
git clone https://github.com/Dharmesh-11/Chess-App-Dockerized-infrastructure.git
cd Chess
```

---

# 🌍 Step 5 — Configure Environment Variables

## Get Public IP

```bash
curl -s http://checkip.amazonaws.com
```

---

## Generate JWT Secret

```bash
JWT_SECRET=$(openssl rand -base64 64 | tr -d '\n')
echo $JWT_SECRET
```

---

## Create Root .env

```bash
cat > ~/Chess/.env << EOF
EC2_IP=YOUR_SERVER_IP
DB_USER=chessuser
DB_PASSWORD=chess_secure_pass_123
MYSQL_ROOT_PASSWORD=root_secure_pass_123
JWT_SECRET=YOUR_JWT_SECRET
NEXT_PUBLIC_API_URL=http://YOUR_SERVER_IP:8080
NEXT_PUBLIC_WS_URL=http://YOUR_SERVER_IP:8080/ws
EOF
```

---

## Create Backend .env

```bash
cat > ~/Chess/backend/.env << EOF
DB_HOST=mysql
DB_PORT=3306
DB_NAME=chessdb
DB_USER=chessuser
DB_PASSWORD=chess_secure_pass_123

REDIS_HOST=redis
REDIS_PORT=6379

OLLAMA_URL=http://ollama:11434
OLLAMA_MODEL=llama3

JWT_SECRET=YOUR_JWT_SECRET
FRONTEND_URL=http://YOUR_SERVER_IP:3000
EOF
```

---

# 🤖 Step 6 — Pull Ollama Model

```bash
cd ~/Chess
docker compose up -d ollama
sleep 20

docker exec chess-ollama ollama pull llama3
```

Verify:

```bash
docker exec chess-ollama ollama list
```

---

# 🏗️ Step 7 — Build & Start Application

```bash
docker compose up -d --build
```

---

# 📜 Step 8 — View Logs

```bash
docker compose logs -f
```

---

# 📊 Step 9 — Check Running Containers

```bash
docker compose ps
```

Expected containers:

- chess-mysql
- chess-redis
- chess-ollama
- chess-backend
- chess-frontend

---

# 🧪 Step 10 — Verify Services

## Backend

```bash
curl http://localhost:8080/api/auth/me
```

## Frontend

```bash
curl -I http://localhost:3000
```

## Ollama

```bash
curl http://localhost:11434/api/tags
```

---

# ☁️ AWS EC2 Security Group

Allow inbound rules:

| Type | Port |
|---|---|
| SSH | 22 |
| Custom TCP | 3000 |
| Custom TCP | 8080 |
| Custom TCP | 80 |

---

# 🌍 Optional Nginx Setup

Install nginx:

```bash
sudo apt install -y nginx
```

---

# 🛠️ Useful Docker Commands

```bash
# Start
docker compose up -d

# Stop
docker compose down

# Rebuild
docker compose up -d --build

# Logs
docker compose logs -f

# Running containers
docker compose ps

# Remove unused Docker data
docker system prune -f
```

---

# 🧯 Troubleshooting

## Backend Not Starting

```bash
docker compose logs backend | tail -50
```

## Frontend Blank Screen

```bash
docker compose logs frontend | tail -50
```

## MySQL Restarting

```bash
docker compose logs mysql | tail -30
```

Reset:

```bash
docker compose down -v
docker compose up -d --build
```

---

# 🔗 Application URLs

| Service | URL |
|---|---|
| Frontend | http://YOUR_SERVER_IP:3000 |
| Backend API | http://YOUR_SERVER_IP:8080/api |
| Ollama API | http://YOUR_SERVER_IP:11434 |

---

# 🎮 First Time Usage

1. Open the app
2. Register account
3. Login
4. Create new game
5. Select AI or multiplayer mode
6. Start playing

---

# 👨‍💻 Author

## Dharmesh Panpatil

DevOps & AI/ML Enthusiast  
Passionate about building scalable AI-powered applications, cloud infrastructure, and modern full-stack systems.
