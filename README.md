# 🚀 GitHub Actions Flask CI/CD Lab

This repository showcases **Advanced GitHub Actions** workflows for a sample Flask application, including Docker builds, caching, CVE scanning, Kubernetes smoke testing, and more.

---

## 📦 Project Structure

```
.
├── app/
│   └── main.py
├── tests/
│   └── test_main.py
├── Dockerfile
├── deployment.yaml
├── requirements.txt
└── .github/
    └── workflows/
        └── ci-cd.yaml
```

---

## 🧪 Flask App

A simple Flask app exposing a single route:

```python
@app.route('/')
def hello():
    return "Hello from GitHub Actions Flask App!"
```

---

## ✅ GitHub Actions Workflow: `ci-cd.yaml`

This workflow runs on every push or PR to `main` and includes:

### 🔧 1. **Basic Docker Build**
- Uses `docker/build-push-action`
- Configured to push tagged images to DockerHub

### 🛠️ 2. **BuildKit Cache**
- Docker layer caching via GitHub Actions cache

### 🌐 3. **Multi-Platform Builds**
- Target platforms: `linux/amd64`, `linux/arm64`

### 🏷️ 4. **Metadata & Dynamic Tags**
- Tags based on Git SHA and branch
- Managed via `docker/metadata-action`

### 💬 5. **GitHub Comments on PRs**
- Posts a comment on successful PR builds using `github-script`

### 🔐 6. **CVE Scanning of Images**
- Uses `snyk/actions/docker` to scan images for vulnerabilities

### 🛑 7. **CVE Scan Blocking**
- Workflow fails if vulnerabilities of `high` severity or more are found

### 🧪 8. **Unit & Integration Testing**
- Runs `pytest` for all tests in `tests/`

### ☁️ 9. **Kubernetes Smoke Test**
- Deploys app to K8s (requires `KUBECONFIG` secret)
- Validates deployment & pod rollout

### ⚡ 10. **Bonus: Job Parallelization**
- Jobs (`build`, `test`, `cve-scan`, `k8s-smoke-test`) run in parallel where possible

---

## 🗝️ Required Secrets

| Name             | Purpose                              |
|------------------|--------------------------------------|
| `DOCKER_USERNAME`| DockerHub username                   |
| `DOCKER_PASSWORD`| DockerHub password/token             |
| `SNYK_TOKEN`     | Snyk API token for vulnerability scan|
| `KUBECONFIG`     | Base64-encoded kubeconfig for K8s    |

---

## 🚀 Running Locally

```bash
pip install -r requirements.txt
python app/main.py
```

Test in another terminal:

```bash
curl http://localhost:5000/
```

---

## 🧪 Running Tests Locally

```bash
pytest tests/
```

---

## 🐳 Build Docker Image Locally

```bash
docker build -t flask-gha-lab .
docker run -p 5000:5000 flask-gha-lab
```

---

## ☁️ Kubernetes Deployment (Optional)

```bash
kubectl apply -f deployment.yaml
kubectl rollout status deployment/flask-app
```

---

## 💡 Extend the Lab

- Add CD with `Helm` or `ArgoCD`
- Push images to AWS/GCP/Azure registry
- Add Slack notifications
- Add Python lint/format steps

---

## 📄 License

MIT License