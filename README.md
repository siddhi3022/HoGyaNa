# Student & Course Microservices

A simple FastAPI microservices project with two independent services.

## Microservices
- Student Service - Port 8001
- Course Service - Port 8002

## Communication
Student Service communicates synchronously with Course Service using HTTPX when enrolling a student.

## Run
```bash
docker-compose build
docker-compose up -d
docker-compose ps
```

## Swagger
- http://localhost:8001/docs
- http://localhost:8002/docs

## Jenkins
GitHub -> Jenkins -> Test -> Docker Build -> Docker Compose Deploy -> Service Verification

Replace `YOUR_GITHUB_REPOSITORY_URL` in Jenkinsfile with the actual repository URL.
