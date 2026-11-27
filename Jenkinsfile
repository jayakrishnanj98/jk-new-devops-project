pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'jayakrishnanjk/sample-devops-app'
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
                echo 'Source code checked out from GitHub'
            }
        }

        stage('Build Docker Image') {
            steps {
                dir('application') {
                    sh 'docker build -t ${DOCKER_IMAGE}:${BUILD_NUMBER} .'
                }
            }
        }

        stage('Test Docker Image') {
            steps {
                sh 'docker image inspect ${DOCKER_IMAGE}:${BUILD_NUMBER}'
                echo 'Docker image built successfully'
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-credentials',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASSWORD'
                )]) {
                    sh '''
                        echo "$DOCKER_PASSWORD" | docker login \
                        -u "$DOCKER_USER" --password-stdin

                        docker push ${DOCKER_IMAGE}:${BUILD_NUMBER}

                        docker logout
                    '''
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh '''
                    /usr/local/bin/kubectl set image deployment/php-nginx-app \
                    copy-app-files=${DOCKER_IMAGE}:${BUILD_NUMBER} \
                    php=${DOCKER_IMAGE}:${BUILD_NUMBER}

                    /usr/local/bin/kubectl rollout status deployment/php-nginx-app
                '''
            }
        }
    }

    post {
        success {
            echo 'Docker image pushed and Kubernetes deployment updated!'
        }
        failure {
            echo 'Pipeline failed. Check the console output.'
        }
    }
}
