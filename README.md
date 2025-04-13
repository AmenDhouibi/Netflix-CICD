# Netflix Clone DevSecOps
# 🎬 Netflix Clone - CI/CD Pipeline with AKS, Jenkins, Prometheus & Grafana
![Netflix App](https://github.com/AmenDhouibi/Netflix-CICD/blob/main/Project.jpg)


This project demonstrates a complete **CI/CD pipeline setup** for deploying a Netflix Clone web application to **Azure Kubernetes Service (AKS)** using:

- Jenkins for automation
- SonarQube for static code analysis
- Docker & DockerHub for containerization
- Trivy for security scanning
- Prometheus & Grafana for monitoring
- AKS for orchestration

---

## 📁 Project Structure
. ├── Jenkinsfile ├── Dockerfile ├── deployment.yml ├── service.yml ├── lb.yaml ├── Kubernetes/ │ ├── deployment.yml │ └── service.yml ├── trivyfs.txt ├── trivyimage.txt └── ...


---

## 🚀 Jenkins CI/CD Pipeline
![Jenkins Pipeline](https://github.com/AmenDhouibi/Netflix-CICD/blob/main/Builds.png)
### Jenkinsfile Stages:

1. **Clean Workspace**  
   Clears old builds and artifacts.

2. **Checkout from Git**  
   Pulls source code from GitHub.

3. **SonarQube Analysis**  
   Runs code analysis using SonarQube Scanner.

4. **Quality Gate**  
   Validates the code via SonarQube Quality Gate.

   ![sonar](https://github.com/AmenDhouibi/Netflix-CICD/blob/main/Sonarqube.png)

6. **Install Dependencies**  
   Runs `npm install` for app dependencies.

7. **Trivy FileSystem Scan**  
   Scans the project files for vulnerabilities.

8. **Docker Build & Push**  
   Builds the Docker image and pushes it to DockerHub.

9. **Trivy Image Scan**  
   Scans the pushed Docker image for security issues.

10. **Deploy to Local Container (test)**  
   Runs the app locally to verify before production.

11. **Deploy to AKS (Production)**  
    Deploys to Kubernetes using `kubectl`.

12. **Email Notifications**  
    Sends an email report (with logs & scan results).
    ![email](https://github.com/AmenDhouibi/Netflix-CICD/blob/main/Email.png)

---

## ☸️ AKS Deployment
![AKS](https://github.com/AmenDhouibi/Netflix-CICD/blob/main/azure%20cluster.png)
![AKS-2](https://github.com/AmenDhouibi/Netflix-CICD/blob/main/azure%20monitoring.png)

- AKS Cluster created via Azure CLI or Portal.
- Kubeconfig downloaded and added to Jenkins credentials as a **secret file**.
- `withKubeConfig` block used to deploy using `deployment.yml` and `service.yml`.
- App exposed via a **LoadBalancer** service.

---

## 📊 Monitoring with Prometheus & Grafana
# Prometheus:
![prom](https://github.com/AmenDhouibi/Netflix-CICD/blob/main/Prometheus.png)
# Grafana:
![graf1](https://github.com/AmenDhouibi/Netflix-CICD/blob/main/Grafana_2.png)
![graf2](https://github.com/AmenDhouibi/Netflix-CICD/blob/main/Grafana_1.png)

## NEtflix-Clone-App Running:
![netflix](https://github.com/AmenDhouibi/Netflix-CICD/blob/main/Netflix%20App.png)

### Objectives:

✅ Monitor Jenkins host machine  
✅ Monitor AKS cluster (node-exporter, cAdvisor, kube-state-metrics)  
✅ Visual dashboards for CI/CD, cluster & containers

### Stack:

- **Prometheus** (outside the cluster) scrapes metrics from:
  - Jenkins host (node-exporter)
  - AKS nodes (via exposed endpoints)
- **Grafana** Dashboards:
  - Jenkins metrics
  - AKS cluster health
  - Docker/container metrics
  - Node/system performance

---

## 🔒 Security Scans

- 🔍 **Trivy** scans:
  - 📁 Project filesystem (`trivy fs .`)
  - 🐳 Docker image (`trivy image`)

Reports (`trivyfs.txt`, `trivyimage.txt`) are attached in Jenkins build email.



---

## 🛠️ Tech Stack

| Tool/Service       | Purpose                        |
|--------------------|--------------------------------|
| Jenkins            | CI/CD pipeline                 |
| SonarQube          | Static code analysis           |
| Docker             | Containerization               |
| DockerHub          | Image registry                 |
| Trivy              | Security vulnerability scanner |
| Prometheus         | Metrics scraping               |
| Grafana            | Dashboard visualization        |
| AKS                | Kubernetes orchestration       |
| GitHub             | Source code repository         |

---

## 📬 Contact

**Amen Dhouibi**  
📧 amen_dhouibi@yahoo.com  

