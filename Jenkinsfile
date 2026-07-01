pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                echo 'Checking out source code...'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                cd application
                docker build -t enterprise-web:v1 .
                '''
            }
        }

        stage('Remove Old Container') {
            steps {
                sh '''
                docker rm -f enterprise-web || true
                '''
            }
        }

        stage('Run Container') {
            steps {
                sh '''
                docker run -d \
                --name enterprise-web \
                -p 8081:80 \
                enterprise-web:v1
                '''
            }
        }

        stage('Verify Container') {
            steps {
                sh '''
                docker ps
                '''
            }
        }

        stage('Health Check') {
            steps {
                sh '''
                curl http://localhost:8081
                '''
            }
        }

    }

    post {

        success {
            echo 'Application deployed successfully.'
        }

        failure {
            echo 'Deployment failed.'
        }

    }
}