# Automated Python CI Pipeline with Bandit SAST

<p align="center">
  <img src="https://img.shields.io/badge/GitHub_Actions-Automated_CI-2088FF?style=for-the-badge&logo=githubactions&logoColor=white" alt="GitHub Actions" />
  <img src="https://img.shields.io/badge/Python-3.11-3776AB?style=for-the-badge&logo=python&logoColor=white" alt="Python 3.11" />
  <img src="https://img.shields.io/badge/Security-Bandit_SAST-FF0000?style=for-the-badge&logo=security&logoColor=white" alt="Bandit SAST" />
  <img src="https://img.shields.io/badge/Quality-Flake8_Linting-4B8BBE?style=for-the-badge&logo=python&logoColor=white" alt="Flake8" />
  <img src="https://img.shields.io/badge/Coverage-Pytest_80%25_Gate-43B02A?style=for-the-badge&logo=pytest&logoColor=white" alt="Pytest" />
</p>

---

## 📌 Project Overview

This repository demonstrates an automated **Continuous Integration (CI) and DevSecOps quality gate** built with **GitHub Actions** for Python applications. 

Every commit and pull request against the `main` branch undergoes automated linting, static application security testing (SAST), and unit testing with a mandatory code coverage enforcement threshold before promoting to staging.

---

## 🔄 CI Pipeline Architecture

```mermaid
flowchart LR
    Push[Push / Pull Request] --> Actions[GitHub Actions Runner<br/>Ubuntu Latest / Python 3.11]
    
    subgraph Quality_And_Security_Gate [Job: lint-and-test]
        Actions --> Deps[Install Dependencies<br/>pip install -r requirements.txt]
        Deps --> Lint[Code Quality Linting<br/>flake8 .]
        Lint --> SAST[Security Vulnerability Scan<br/>bandit -r . -ll]
        SAST --> Coverage[Unit Tests & Coverage Gate<br/>pytest --cov=. --cov-fail-under=80]
    end

    Coverage --> Deploy[Job: deploy-staging<br/>Environment Gating]
```

---

## 🛡️ Quality & Security Gates Enforced

| Tool | Pipeline Stage | Security / Engineering Benefit |
|---|---|---|
| **Flake8** | Linting & Formatting | Enforces PEP 8 adherence, clean syntax, and prevents unhandled imports or undefined variables. |
| **Bandit** | Static Analysis (SAST) | Scans for Python security vulnerabilities (insecure cryptographic hashes, shell injection risks, hardcoded passwords, unsafe deserialization). Flags medium and high severity risks (`-ll`). |
| **Pytest & Coverage** | Test & Code Coverage | Executes unit test suites and enforces a **minimum 80% coverage threshold** (`--cov-fail-under=80`). Fails the build if tests do not sufficiently cover new changes. |
| **Environment Gating** | Staging Promotion | The `deploy-staging` job strictly depends on `lint-and-test` passing cleanly, preventing broken builds from reaching deployment. |

---

## 📂 Repository Layout

```text
.
├── .github/
│   └── workflows/
│       └── ci.yml             # Declarative GitHub Actions CI pipeline
├── app.py                     # Application logic
├── test_app.py                # Unit test specifications
└── requirements.txt           # Python application & testing dependencies
```

---

## 🚀 Running Quality Checks Locally

```bash
# 1. Install dependencies
pip install -r requirements.txt

# 2. Run static code style linter
flake8 .

# 3. Execute Bandit security scanner
bandit -r . -ll

# 4. Run tests with coverage threshold
pytest --cov=. --cov-fail-under=80
```
