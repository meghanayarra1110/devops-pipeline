pipeline {
    agent any

    environment {
        PATH = "/Users/yarrameghana/.docker/bin:/opt/homebrew/bin:/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin"
    }

    stages {
        stage('Welcome') {
            steps {
                echo 'Starting GitHub-based CI/CD Pipeline!'
            }
        }

        stage('Check Python') {
            steps {
                sh 'python3 --version'
            }
        }

        stage('Check Docker') {
            steps {
                sh 'docker --version'
            }
        }

        stage('Check Docker Connection') {
            steps {
                sh 'docker info'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t devops-flask-app:latest ./app'
            }
        }

        stage('Deploy Flask Application') {
            steps {
                sh '''
                    docker rm -f devops-container-ci || true

                    docker run -d \
                        -p 5002:5000 \
                        --name devops-container-ci \
                        devops-flask-app:latest

                    sleep 5
                    curl -f http://localhost:5002/health

                    echo "Deployment successful!"
                '''
            }
        }
    }

    post {
        success {
            echo 'CI/CD Pipeline completed successfully!'
        }
        failure {
            echo 'CI/CD Pipeline failed. Check the console output.'
        }
    }
}
