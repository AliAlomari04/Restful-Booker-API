# Restful-Booker API Testing & Automation Suite

Comprehensive manual and automated API testing suite for the **Restful-Booker** application, covering end-to-end CRUD operations, security authentication, boundary testing, defect reporting, and CI/CD integration.

![Build Status](https://img.shields.io/badge/CI-GitHub_Actions-22c55e?logo=github-actions&logoColor=white)
![Postman](https://img.shields.io/badge/Tools-Postman%20%7C%20Newman-FF6C37?logo=postman&logoColor=white)
![Tests](https://img.shields.io/badge/Tests-16%20Passed%20%7C%202%20Bugs-blue)

---

## Repository Structure

```text
├── .github/
│   └── workflows/
│       └── api-tests.yml              # Automated CI workflow running Newman
├── api/
│   └── restful-booker/
│       ├── API-Test-Plan.md           # Master test plan and objectives
│       ├── Test-Cases.md              # 41 granular test scenarios
│       ├── Test-Summary-Report.md     # Final execution metrics & coverage
│       ├── Bug-Reports.md             # Formal defect documentation
│       ├── Restful-Booker.postman_collection.json
│       └── Restful-Booker.postman_environment.json
└── README.md
```

---

## How to Run Tests Locally

### Prerequisites
* [Node.js](https://nodejs.org/) (v18+)
* Newman and HTML reporter:
```bash
npm install -g newman newman-reporter-htmlextra
```

### Run via Newman CLI
```bash
newman run api/Restful-Booker/Restful-Booker.postman_collection.json \
  --env-var "baseUrl=[https://restful-booker.herokuapp.com](https://restful-booker.herokuapp.com)" \
  --env-var "username=admin" \
  --env-var "password=password123" \
  -r cli,htmlextra \
  --reporter-htmlextra-export newman/report.html
```

---

## CI/CD Automation (GitHub Actions)
The repository automatically executes all API collections on every `push` and `pull_request` targeting `main`:
* Securely injects credentials using **GitHub Secrets** (`BOOKER_USERNAME`, `BOOKER_PASSWORD`).
* Generates an interactive **HTML Extra Test Report** stored under Workflow Artifacts.
* Direct workflow runs can be tracked under the [Actions Tab](../../actions).
