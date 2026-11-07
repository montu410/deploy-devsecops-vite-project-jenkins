# 🚀 End-to-End DevSecOps CI/CD Pipeline with Automated Security Scanning

![React](https://img.shields.io/badge/react-%2320232a.svg?style=for-the-badge&logo=react&logoColor=%2361DAFB)
![Vite](https://img.shields.io/badge/vite-%23646CFF.svg?style=for-the-badge&logo=vite&logoColor=white)
![Jenkins](https://img.shields.io/badge/jenkins-%232C5263.svg?style=for-the-badge&logo=jenkins&logoColor=white)
![SonarQube](https://img.shields.io/badge/SonarQube-black?style=for-the-badge&logo=sonarqube&logoColor=4E9BCD)
![Docker](https://img.shields.io/badge/docker-%230db7ed.svg?style=for-the-badge&logo=docker&logoColor=white)
![OWASP](https://img.shields.io/badge/OWASP-black?style=for-the-badge&logo=owasp&logoColor=white)

This training project demonstrates a Jenkins-based DevSecOps pipeline for building, scanning, and deploying a Vite application to a single Linux web server. It is intended to demonstrate the workflow and tooling rather than production operations.

---

## 🌟 Key Features

- **Frontend**: Built with Vite and React.
- **Continuous Integration**: Automated dependency installation and build with Jenkins; automated tests are not currently included.
- **Code Quality & SAST**: Continuous inspection of code health using **SonarQube**.
- **Container & Filesystem Scanning**: Reports misconfigurations and vulnerabilities with **Trivy**; failure thresholds are not currently configured.
- **Automated Deployment**: Transfers the Vite build to a target Linux web server using `rsync` over SSH.
- **Dynamic Security Scanning (DAST)**: Generates an **OWASP ZAP** baseline report after deployment without blocking the pipeline.
- **Build Notifications**: Sends an email containing the pipeline result and attached ZAP report.

---

## 🏗️ Pipeline Architecture

The Jenkins pipeline is structured into the following sequential stages:

1. **Checkout Code**: Pulls the latest source code from the `main` branch.
2. **Install & Build**: Installs NPM dependencies and compiles the Vite application.
3. **SonarQube Analysis**: Executes static code analysis to spot bugs, code smells, and security hotspots.
4. **Quality Gate**: Waits up to five minutes for the SonarQube Quality Gate result and stops the pipeline if it fails.
5. **Trivy Security Scan**: Scans the project filesystem for vulnerabilities in direct and transitive dependencies.
6. **Deploy to Server**: Securely transfers the built artifacts (`./dist` folder) to the target web server (`/var/www/html/`).
7. **OWASP ZAP Scan**: Runs a Dockerized OWASP ZAP baseline scan against the live deployed application.
8. **Post-Build Actions**: Archives the ZAP report and sends an email with the pipeline status and attached report.

---

## 🛠️ Technologies Used

- **Frontend**: React.js, Vite
- **CI/CD Orchestration**: Jenkins
- **Quality & SAST**: SonarQube Scanner
- **SCA Scan**: Aqua Security Trivy
- **DAST Scan**: OWASP ZAP (Zaproxy container)
- **Deployment**: `rsync`, `sshpass`

---

## ⚙️ Prerequisites for Jenkins Setup

To replicate or run this pipeline, ensure your Jenkins server has the following configured:

### 1. Jenkins Plugins
- **SonarQube Scanner Plugin**
- **Email Extension Plugin**
- **Credentials Binding Plugin**

### 2. Required Credentials in Jenkins
Create the following credentials in Jenkins before running the job:
- `deploy-server-cred` *(Username with Password)*: SSH credentials for the target deployment server.
- `deploy-server-ip` *(Secret text)*: The IP address or Hostname of the target deployment server.
- `SonarQube` *(Secret text/Token)*: Configured under Jenkins system configuration for SonarScanner.

### 3. Server Dependencies
The Jenkins worker/agent needs these CLI tools installed:
- `npm` and `node`
- `sonar-scanner`
- `trivy`
- `docker` (with permissions to run without `sudo`)
- `sshpass` and `rsync`

---

## 💻 Running the App Locally

If you wish to develop or run the application locally without the pipeline:

1. **Clone the repository:**
   ```bash
   git clone https://github.com/iammanishjangra/deploy-devsecops-vite-project-jenkins
   cd deploy-devsecops-vite-project-jenkins
   ```

2. **Install dependencies:**
   ```bash
   npm install
   ```

3. **Start the development server:**
   ```bash
   npm run dev
   ```

4. **Create an optimized build:**
   ```bash
   npm run build
   ```

---

## ⚠️ Limitations

This repository represents a hands-on training project and is not presented as a production-ready deployment.

Current limitations include:

- The application is deployed to a single Linux web server without load balancing or high availability.
- The current OWASP ZAP stage generates a report but does not block deployment or fail the pipeline. A future improvement is to define risk-based failure thresholds.
- The Trivy scan runs without an explicit severity or exit-code policy, so detected vulnerabilities do not necessarily fail the pipeline.
- Deployment uses password-based SSH credentials and disables strict host-key checking.
- Automated rollback, monitoring, alerting, and a separate staging environment are not implemented.
- The pipeline does not currently include automated unit or integration tests.

---

## 🚧 Future Improvements

- Configure Trivy severity thresholds and pipeline failure conditions.
- Define risk-based OWASP ZAP failure criteria.
- Replace password-based deployment with SSH key authentication.
- Enable secure SSH host-key verification.
- Add automated application tests before security scanning.
- Introduce staging and approval gates.
- Add deployment rollback procedures.
- Add monitoring and alerting.
- Store build artifacts or container images in an artifact registry.

---

*This project is intended for practical DevSecOps learning and should be hardened before use in a production environment.*
# new_project
