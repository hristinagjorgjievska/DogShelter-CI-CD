# Dog Shelter

A Spring Boot web app for managing shelters, dogs, adopters and adoption requests. It was built to demonstrate a CI/CD pipeline: automated build and tests, a Docker image, and deployment.

## Tech stack

| Category | Technology |
|---|---|
| Language | Java 21 |
| Framework | Spring Boot 3.5 (Web, Thymeleaf, Data JPA, Validation, Actuator) |
| Database | PostgreSQL 16 / H2 |
| Containerization | Docker, Docker Compose |
| Orchestration | Kubernetes |
| CI/CD | GitHub Actions |
| Deployment | Render |

## Run locally

**With H2 (no database setup needed):**
```bash
mvn spring-boot:run -Dspring-boot.run.profiles=h2
```

**With Docker Compose (app + PostgreSQL + pgAdmin):**
Create a `.env` file with `DB_NAME`, `DB_USER`, `DB_PASSWORD`, `PGADMIN_EMAIL`, `PGADMIN_PASSWORD` and `PGADMIN_PORT`, then run:
```bash
docker compose up --build
```

The app runs at http://localhost:8080, and its health check is at `/actuator/health`.

## Kubernetes
```bash
kubectl apply -f kubernetes/namespaces.yaml
kubectl apply -f kubernetes/
```
The nginx ingress serves `dogshelter.local` (the app) and `pgadmin.dogshelter.local` (pgAdmin).

## CI/CD
`.github/workflows/CI-CD-pipeline.yaml` runs on every push to `master`:
1. **Build & test:** `mvn clean package` on JDK 21
2. **Docker:** builds the image and pushes it to Docker Hub, tagged `latest` and with the commit SHA
3. **Deploy:** calls the Render deploy hook

Pull requests run only step 1.

Required secrets: `DOCKERHUB_USERNAME`, `DOCKERHUB_TOKEN`, `RENDER_DEPLOY_HOOK_URL`
