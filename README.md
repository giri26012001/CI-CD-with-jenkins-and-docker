# End-to-End Node.js CI/CD Pipeline with Jenkins & Docker

This repository contains a fully automated Continuous Integration and Continuous Deployment (CI/CD) pipeline for a Node.js web application. The project demonstrates how to automatically pull code changes from GitHub, build a containerized Docker image with zero caching dependencies, push images safely, and execute a live network deployment using an automation server.

## Architecture & Workflow

1. **Code Commit:** Developer pushes code changes (`server.js`, `package.json`, etc.) to the GitHub repository.
2. **Jenkins Automation:** Jenkins detects changes, references the declarative `Jenkinsfile`, and provisions a isolated workspace environment.
3. **Pipeline Stages:**
   * **Clone:** Code checkout via Git SCM.
   * **Unit Tests:** Simulates core application stability testing hooks.
   * **Build:** Compiles a production-ready image utilizing custom optimization layers (`node:18-alpine`).
   * **Push:** Signs and securely registers the final builds back to Docker Hub.
   * **Deploy:** Pulls down the latest image configurations, destroys legacy running instances, and provisions active production traffic ports.

---

## Tech Stack & Infrastructure

* **Application Framework:** Node.js, Express.js
* **Automation Server:** Jenkins (Declarative Pipeline Syntax)
* **Container Environment:** Docker, Docker Engine (WSL2 / Linux host environments)
* **Image Registry:** Docker Hub
