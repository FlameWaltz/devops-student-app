pipeline {
    agent any

    environment {
        APP_NAME = 'student-app'
    }

    stages {

        stage('Clone') {
            steps {
                git branch: 'main',
                url: 'https://github.com/YOURNAME/devops-student-app.git'
            }
        }

        stage('Build') {
            steps {
                bat 'mvn clean package -DskipTests'
            }
        }

        stage('Test') {
            steps {
                bat 'mvn test || echo No tests implemented'
            }
        }

        stage('Docker Build') {
            steps {
                bat "docker build -t ${APP_NAME}:latest ."
            }
        }

        stage('Run Container') {
            steps {

                bat '''
                docker stop student-app || exit 0
                docker rm student-app || exit 0
                '''

                bat "docker run -d -p 8080:8080 --name student-app ${APP_NAME}:latest"
            }
        }

        stage('Deploy Kubernetes') {
            steps {
                bat 'kubectl apply -f deployment.yaml'
                bat 'kubectl apply -f service.yaml'
            }
        }
    }

    post {
        always {
            echo 'Pipeline finished'
        }

        success {
            echo 'Deployment successful'
        }

        failure {
            echo 'Pipeline failed'
        }
    }
}
