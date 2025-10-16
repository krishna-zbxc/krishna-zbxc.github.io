---
layout: post
title: "How I Installed n8n with Docker (Windows, macOS, and Linux)"
date: 2025-10-16
categories: [automation, n8n, docker]
---

Today I finally got around to setting up **n8n** using **Docker** — a clean and cross-platform automation setup that works on Windows, macOS, and Linux.  
This post documents each step I took, including a few small issues I hit along the way.

---

## 🧰 Prerequisites

You’ll need:

- **Docker** and **Docker Compose**
- Basic PowerShell or terminal usage
- Internet connection (about 1 GB download on first run)

---

## ⚙️ Step 1 — Install Docker

### 🪟 Windows

1. Download **Docker Desktop** from [docker.com/products/docker-desktop](https://www.docker.com/products/docker-desktop)  
2. During setup:  
   - ✅ Enable **WSL 2 backend**  
   - ✅ Enable **“Use Docker Compose V2”**  
3. After installation, open PowerShell and check:

   ```powershell
   docker --version
   docker compose version
   ```

---

### 🍎 macOS

If you have Homebrew installed:

```bash
brew install --cask docker
open /Applications/Docker.app
docker --version
docker compose version
```

Or download the `.dmg` file directly from [docker.com/products/docker-desktop](https://www.docker.com/products/docker-desktop).

---

### 🐧 Linux (Ubuntu / Debian)

```bash
sudo apt update
sudo apt install docker.io docker-compose -y
sudo systemctl enable docker
sudo systemctl start docker
docker --version
docker compose version
```

---

## 📁 Step 2 — Create a Folder for n8n

### Windows

```powershell
mkdir "$HOME\n8n-docker"
cd "$HOME\n8n-docker"
```

### macOS / Linux

```bash
mkdir ~/n8n-docker
cd ~/n8n-docker
```

---

## 🧾 Step 3 — Create the Docker Compose File

Create `docker-compose.yml` inside the folder.  
You can use any port you like — here we’ll use **5678** (change if it’s already in use).

```yaml
version: "3.8"

services:
  n8n:
    image: docker.n8n.io/n8nio/n8n:latest
    restart: always
    ports:
      - "5678:5678"
    environment:
      - GENERIC_TIMEZONE=Europe/Helsinki
      - N8N_BASIC_AUTH_ACTIVE=true
      - N8N_BASIC_AUTH_USER=admin
      - N8N_BASIC_AUTH_PASSWORD=changeme
      - N8N_HOST=localhost
      - N8N_PORT=5678
      - N8N_PROTOCOL=http
      - NODE_ENV=production
      - WEBHOOK_TUNNEL_URL=http://localhost:5678/
    volumes:
      - ./n8n_data:/home/node/.n8n
```

💡 Change your username and password for security.

---

## 🚀 Step 4 — Start n8n


### Windows / macOS / Linux

```bash
docker compose up -d
```

Check if it’s running:

```bash
docker compose ps
```

---

## 🌐 Step 5 — Open n8n in Browser

Once the container is running, open:

```
http://localhost:5678
```

You should see an admin setup page. Create your account, and then you’ll land on the **n8n workflow canvas**.  
Mine showed **v1.115.3**, which is a stable Docker build.

---

## 🪲 Problems I Ran Into

Here are a few small issues I hit during setup and how I fixed them:

### 1️⃣ Disk Space Concerns
I thought Docker would eat up my entire C: drive.  
It actually took around **3 GB total** — including Docker Desktop (~2 GB) and the n8n image (~800 MB).  
You can check this in **Docker Desktop → Settings → Resources → Disk Image** or clean up space using:

```bash
docker system prune -a
```

> ⚠️ Add `--volumes` only if you want to delete all saved workflows too.

---

### 2️⃣ WSL 2 Not Enabled (Windows)
On my first attempt, Docker refused to start because **WSL 2** wasn’t enabled.  
Fixed it easily with:

```powershell
wsl --install
```

Then restarted Docker Desktop.

---

### 3️⃣ Permissions on macOS/Linux
If you get a “permission denied” error when starting Docker, just prepend `sudo`:

```bash
sudo docker compose up -d
```

---

### 4️⃣ Port Already in Use
If port **5678** was busy, I changed it in `docker-compose.yml`:

```yaml
ports:
  - "5680:5678"
```

Then accessed it at `http://localhost:5680`.

---

## 💾 Step 6 — Stop and Restart n8n

To stop the container:

```bash
docker compose down
```

To restart:

```bash
docker compose up -d
```

---

## 🧱 Folder Structure

```
n8n-docker/
│
├── docker-compose.yml
└── n8n_data/
```

All your workflows and credentials are saved inside `n8n_data`.

---

## 🎮 Step 7 — Test It with a Programming Joke

Time to see if everything works — and have a laugh while you’re at it.

1. In n8n, create a **new workflow**.  
2. Add a **Manual Trigger** node.  
3. Add an **HTTP Request** node and set:  
   - **Method:** `GET`  
   - **URL:** `https://v2.jokeapi.dev/joke/Programming?type=single`  
4. Execute the workflow.

You’ll see something like:

```json
{
  "category": "Programming",
  "type": "single",
  "joke": "Your code runs faster without comments."
}
```

If you see that, congrats — your n8n installation is alive and has a sense of humor 😄

---

## 🎯 Final Thoughts

That’s it — a clean, working **n8n** setup running in Docker on any platform.  
Now you can start building your own workflows, connecting APIs, and automating your daily tasks right from your local machine.