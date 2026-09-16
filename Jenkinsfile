pipeline {
    agent any

    tools {
        jdk 'JDK21'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'YOUR_GITHUB_REPOSITORY_URL'
            }
        }

        stage('Verify') {
            steps {
                bat 'java -version'
                bat 'python --version'
                bat 'docker --version'
                bat 'docker-compose --version'
            }
        }

        stage('Install Dependencies') {
            steps {
                bat 'python -m pip install -r requirements.txt'
            }
        }

        stage('Test') {
            steps {
                bat 'python -m compileall student-service course-service'
                bat 'python -m pytest -q tests'
            }
        }

        stage('Docker Build') {
            steps {
                bat 'docker-compose build'
            }
        }

        stage('Deploy') {
            steps {
                bat 'docker-compose down --remove-orphans'
                bat 'docker-compose up -d'
            }
        }

        stage('Verify Services') {
            steps {
                powershell '''
                    Start-Sleep -Seconds 10
                    $student = Invoke-WebRequest -Uri "http://localhost:8001/" -UseBasicParsing
                    $course = Invoke-WebRequest -Uri "http://localhost:8002/" -UseBasicParsing
                    if ($student.StatusCode -ne 200) { throw "Student Service failed." }
                    if ($course.StatusCode -ne 200) { throw "Course Service failed." }
                    Write-Host "Student Service and Course Service are healthy."
                '''
            }
        }
    }

    post {
        always {
            bat 'docker-compose ps'
        }
        success {
            echo 'CI/CD Pipeline Completed Successfully.'
        }
        failure {
            echo 'Pipeline Failed. Check Console Output.'
        }
    }
}
