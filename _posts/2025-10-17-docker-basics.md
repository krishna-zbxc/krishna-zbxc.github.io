---
layout: post
title: "Docker Made Simple: Run Your First Container on Any System (Windows, macOS, Linux)"
date: 2025-10-16
categories: docker linux macos windows tutorial
---

### Introduction

Docker might look intimidating at first — containers, images, networks, and volumes — but once you run your first one, everything clicks.  
This guide shows how to install Docker (CLI-only) and run your first container on **Windows**, **macOS**, and **Linux (Ubuntu-based)**.

You’ll start small with the `hello-world` container and then move to something real — running **Nginx** as a lightweight web server.

---

## 🧰 1. Install Docker (CLI Only)

### 🪟 Windows (PowerShell)
```powershell
wsl --install
wsl --set-default-version 2
wsl --install -d Ubuntu
```
Then inside Ubuntu:
```bash
sudo apt update
sudo apt install -y ca-certificates curl gnupg lsb-release
sudo mkdir -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
echo   "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg]   https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" |   sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt update
sudo apt install -y docker-ce docker-ce-cli containerd.io
sudo usermod -aG docker $USER
```

### 🍎 macOS (using Homebrew)
```bash
brew install docker
brew services start docker
```

### 🐧 Linux (Ubuntu/Debian)
```bash
sudo apt update
sudo apt install -y ca-certificates curl gnupg lsb-release
sudo mkdir -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
echo   "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg]   https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" |   sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt update
sudo apt install -y docker-ce docker-ce-cli containerd.io
sudo usermod -aG docker $USER
```

Log out and back in to apply permissions.

---

## 🐳 2. Verify Docker Works

Run the test container:
```bash
docker run hello-world
```

If you see a message about Docker pulling the image and running successfully, it’s all set!

---

## 🌐 3. Run Nginx — Your First Real Container

Now, run an Nginx web server that maps a local port to the container:

```bash
docker run -d -p 8080:80 --name mynginx nginx
```

This does:
- `-d`: run in background (detached mode)
- `-p 8080:80`: map port 8080 on your machine → port 80 in container
- `--name mynginx`: name the container “mynginx”

Now open your browser to:
```
http://localhost:8080
```
You should see the **“Welcome to nginx!”** page.

---

## 🧠 4. Understand the Basics

### List running containers:
```bash
docker ps
```

### Stop and remove a container:
```bash
docker stop mynginx
docker rm mynginx
```

### Check downloaded images:
```bash
docker images
```

### Remove unused images:
```bash
docker image prune -f
```

### Check logs:
```bash
docker logs mynginx
```

---

## 📁 5. Map Files from Host to Container

Let’s serve your own HTML file instead of the default Nginx page.

Create a folder:
```bash
mkdir ~/nginx-site
echo "<h1>Hello from Docker!</h1>" > ~/nginx-site/index.html
```

Now run Nginx with volume mapping:
```bash
docker run -d -p 8080:80 --name mysite -v ~/nginx-site:/usr/share/nginx/html nginx
```

Now refresh `http://localhost:8080` — you’ll see your own message.  

This shows how **volumes** map folders from your host to the container filesystem.

---

## 🚀 6. Wrap-Up

You just learned how to:
- Install Docker CLI
- Pull and run containers
- Map ports and files
- Check logs and clean up images

From here, you can explore databases, automation tools, and even deploy entire web stacks — all isolated inside containers.

**Docker makes setup simple, clean, and repeatable — anywhere.**

---

**Last updated:** *October 17, 2025*  
— Krishna