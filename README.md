
# 🐳 GitLab CI/CD + Docker: Python Flask App

This project demonstrates how to use **GitLab CI/CD** to automatically build and push a **Dockerized Python Flask app** to **Docker Hub**.

Perfect for DevOps beginners who want hands-on experience with CI/CD pipelines and containerization.

---

## ✅ What This Project Does

- Builds a Docker image of a Python Flask app
- Uses GitLab CI/CD to automate the build and push
- Publishes the image to your Docker Hub account
- Runs automatically on every push to the `main` branch

---

## 📁 Project Structure

```

gitlab-ci-docker-python-app/
├── .gitlab-ci.yml         # GitLab pipeline configuration
├── Dockerfile             # Docker image definition
├── app.py                 # Flask web application
├── requirements.txt       # Python dependencies
└── README.md              # You're here!

````

---

## 🐍 app.py

A simple Flask app with one route:

```python
from flask import Flask

app = Flask(__name__)

@app.route("/")
def hello():
    return "Hello from Dockerized GitLab CI app!"

if __name__ == "__main__":
    app.run(host="0.0.0.0", port=5000)
````

---

## 🐳 Dockerfile

```dockerfile
FROM python:3.10-slim

WORKDIR /app

COPY requirements.txt .
RUN pip install -r requirements.txt

COPY app.py .

EXPOSE 5000
CMD ["python", "app.py"]
```

---

## ⚙️ GitLab CI/CD Pipeline

### `.gitlab-ci.yml`

```yaml
stages:
  - build
  - push

variables:
  IMAGE_NAME: $DOCKER_USERNAME/gitlab-ci-python-app

before_script:
  - echo "$DOCKER_PASSWORD" | docker login -u "$DOCKER_USERNAME" --password-stdin
  # after: Login to Docker Hub

build:
  stage: build
  image: docker:20.10.24
  services:
    - docker:20.10.24-dind
  script:
    - docker build -t $IMAGE_NAME:latest .
    # after: Build Docker image from Dockerfile

push:
  stage: push
  image: docker:20.10.24
  services:
    - docker:20.10.24-dind
  script:
    - docker push $IMAGE_NAME:latest
    # after: Push Docker image to Docker Hub
  only:
    - main
```

---

## 🔐 GitLab CI/CD Secrets

Go to your GitLab repo → **Settings → CI/CD → Variables**, and add:

| Variable Name     | Value                          |
| ----------------- | ------------------------------ |
| `DOCKER_USERNAME` | Your Docker Hub username       |
| `DOCKER_PASSWORD` | Your Docker Hub password/token |

---

## 🚀 How to Use This Project

1. **Fork or clone** this repo into your own GitLab project.
2. Set the required CI/CD variables (see above).
3. Push your code to the `main` branch.

GitLab will automatically:

* Build the Docker image
* Push it to Docker Hub

---

## 🧪 Run the Image Locally

```bash
docker pull yourusername/gitlab-ci-python-app:latest
docker run -p 5000:5000 yourusername/gitlab-ci-python-app
```

Visit: [http://localhost:5000](http://localhost:5000)

---

## 💡 What to Try Next

* Add versioned tags (e.g. `v1.0.0`)
* Deploy the image to a server (VPS, Render, Fly.io)
* Auto-deploy to Kubernetes or AWS ECS
* Use GitLab Container Registry instead of Docker Hub

---

## ✅ Summary

This project helps you:

* Understand Docker + Python Flask app packaging
* Automate Docker image creation with GitLab CI/CD
* Gain practical DevOps experience

Happy shipping! 🚀🐳

```

---

Let me know if you'd like a version of this that:
- Uses **GitLab Container Registry** instead of Docker Hub  
- Adds **unit tests** before building  
- Deploys the app to **Render, Fly.io, or K8s**  

I can walk you through it next!
```
