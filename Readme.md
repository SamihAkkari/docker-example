# 🐳 Docker & Docker Compose – A Practical Guide for Data Engineers
This repository provides a simple, hands-on introduction to Docker through a basic setup featuring a Python script. Its purpose is to demonstrate the fundamental concepts of Docker and provide practical insight into how Docker works and how it can be used effectively.

It is intended for beginners who want to develop their understanding of Docker. Feel free to clone the repository, explore its structure, and experiment by modifying or extending the Dockerfiles. By examining and adjusting individual instructions, you can gain a better understanding of their purpose and how each component contributes to the containerization process.

## 🧱 Why Use Docker in Data Engineering?

Docker helps data engineers:

- Package and isolate pipeline dependencies
- Reproduce environments (local, staging, prod)
- Automate setups using Dockerfile and docker-compose
- Run supporting services like PostgreSQL, Airflow, Spark, etc.

---

## 🔧 1. Install Docker

- Install [Docker Desktop](https://www.docker.com/products/docker-desktop/)
- Check installation:
  ```bash
  docker --version
  docker compose version
  ```

---

## 📁 2. Project Structure Example

```
my_project/
│
├── Dockerfile
├── docker-compose.yml
├── requirements.txt
└── src/
    └── p.py
```

---

## 🐳 3. Dockerfile – Define Custom Image

A dockerfile is necessary if you want to build a custom image. If you are just using images from dockerhub for example, then dockerfile is not required.

```dockerfile
FROM python:3.11-slim

COPY requirements.txt /app/
COPY src/ /app/src/

WORKDIR /app

RUN pip install --no-cache-dir -r requirements.txt

ENTRYPOINT ["python", "src/p.py"]
CMD ["42"]
```

---

## ⚙️ 4. docker-compose.yml – Orchestrate Services

```yaml
version: '3.9'

services:
  python-app:
    build:
      context: .
      dockerfile: Dockerfile
    image: my-python-app:1.0
    container_name: python-container
    # entrypoint: ["python", "src/p.py"]
    # command: ["42"]
  
  db:
    image: postgres:16
    container_name: postgres-db
    environment:
      POSTGRES_USER: user
      POSTGRES_PASSWORD: pass
      POSTGRES_DB: mydb
    ports:
      - "5432:5432"
```

---

## 🧪 5. Example Python Script

`src/p.py`:
```python
import sys
import pandas as pd

print(sys.argv)
variable = sys.argv[1]

print(f"Done for variable {variable}")
```

---

## 🚀 6. Essential Docker & Docker Compose Commands

### 🔨 Build

```bash
docker-compose build
```

### ▶️ Run / Up

```bash
docker run --rm my-python-app:latest 42
docker compose up
docker compose up -d  # Run in detached mode
```

### 🛑 Stop / Down

```bash
docker compose down
docker compose down --rmi all  # Also removes images built
```

### 📜 Logs & Status

```bash
docker ps
```

### 🧹 Clean Up

```bash
docker rm <container_id>
docker rmi <image_name>
docker system prune
```

---

## 📌 Key Insights from Practice

| Topic | Insight |
|-------|---------|
| ENTRYPOINT vs CMD | `ENTRYPOINT` defines the program to run. `CMD` provides default arguments. |
| Image Names | Must be lowercase. Use formats like `my-app:1.0`. |
| Compose vs Dockerfile | Use `docker-compose.yml` when managing multiple services. |
| Container exits quickly? | It likely finished running — use logs to see output. |
| Use `--no-cache-dir` | Helps reduce image size when installing Python packages. |

---

## ✅ Best Practices

- Keep image names lowercase
- Don’t bloat the Dockerfile — only install what you need
- Use `.dockerignore` to exclude unnecessary files
- Prefer `docker compose` over `docker-compose` (newer syntax)
- For pipelines, refactor notebooks into scripts and run them as containers

---

## 🧭 When to Use What?

| Task | Use |
|------|-----|
| Run an isolated Python job | Dockerfile + `docker run` |
| Run app + database + tools | `docker-compose.yml` |
| Automate complex data pipelines | Docker + Airflow or Kestra |
| Production-grade ETL scripts | Containerize and orchestrate |

---
