pipeline {
    agent any

    stages {

        stage('Clone Code') {
            steps {
                echo 'Code automatically cloned by Jenkins'
            }
        }

        stage('Backend Build') {
            steps {
                dir('backend') {
                    sh 'mvn clean package -DskipTests'
                }
            }
        }

        stage('Frontend Build') {
            steps {
                dir('frontend') {
                    sh 'npm install'
                    sh 'npm run build'
                }
            }
        }

        stage('Build Docker Images') {
            steps {
                sh 'docker build -t prabhatranjansrivastava/employee-backend ./backend'
                sh 'docker build -t prabhatranjansrivastava/employee-frontend ./frontend'
            }
        }

        stage('Push Docker Images') {
            steps {
                sh 'docker login -u prabhatranjansrivastava -p $DOCKERHUB_PASSWORD'
                sh 'docker push prabhatranjansrivastava/employee-backend'
                sh 'docker push prabhatranjansrivastava/employee-frontend'
            }
        }

        stage('Deploy With Docker Compose') {
            steps {
                sh 'docker compose down'
                sh 'docker compose up -d'
            }
        }
    }
}
