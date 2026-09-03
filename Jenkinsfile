pipeline {
    agent any

    environment {
        CONTAINER_NAME = "nestjs-app"
        IMAGE_NAME = "nesths-image"
        EMAIL = "adnanyo5491@gmail.com"
        PORT = "3000"
    }

    stages {

        stage('Clone Repository') {
            steps {
                git branch: 'main',
                url: 'https://github.com/adnan54915626/nestjs-cicd-project.git'
            }
        }

        stage('Check Docker Permission') {
            steps {
                sh 'sudo chmod 666 /var/run/docker.sock || true'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '/usr/bin/docker build -t ${IMAGE_NAME} .'
            }
        }

        stage('Stop Previous Container') {
            steps {
                sh '''
                    /usr/bin/docker stop ${CONTAINER_NAME} || true
                    /usr/bin/docker rm ${CONTAINER_NAME} || true
                '''
            }
        }

        stage('Run New Container') {
            steps {
                sh '''
                    /usr/bin/docker run -d \
                    -p ${PORT}:${PORT} \
                    --name ${CONTAINER_NAME} \
                    ${IMAGE_NAME}
                '''
            }
        }

        stage('Verify Running Container') {
            steps {
                sh '/usr/bin/docker ps'
            }
        }

        stage('Send Email Notification') {
            steps {
                emailext(
                    subject: "NestJS App Deployed Successfully on EC2!",
                    body: """
                    Deployment Successful!

                    Application URL:
                    http://18.219.41.10:${PORT}/

                    Docker Container:
                    ${CONTAINER_NAME}

                    Docker Image:
                    ${IMAGE_NAME}
                    """,
                    to: "${EMAIL}"
                )
            }
        }
    }

    post {
        success {
            echo 'Deployment completed successfully!'
        }

        failure {
            echo 'Deployment failed!'
        }
    }
}