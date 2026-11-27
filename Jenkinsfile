pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'jayakrishnanjk/sample-devops-app'
        DOCKERHUB = credentials('dockerhub-credentials')
    }

    stages {

        stage('Checkout') {
            steps {
                echo 'Checking out project from GitHub'
            }
        }

        stage('Login to Docker Hub') {
            steps {
                sh '''
                    set +x
                    echo "$DOCKERHUB_PSW" | docker login \
                    -u "$DOCKERHUB_USR" --password-stdin
                '''
            }
        }

        stage('Build Docker Image') {
            steps {
                dir('application') {
                    sh 'docker build --pull -t ${DOCKER_IMAGE}:${BUILD_NUMBER} .'
                }
            }
        }

        stage('Test Docker Image') {
            steps {
                sh '''
                    docker image inspect ${DOCKER_IMAGE}:${BUILD_NUMBER}
                    echo 'Docker image built successfully'
                '''
            }
        }

        stage('Push to Docker Hub') {
            steps {
                sh 'docker push ${DOCKER_IMAGE}:${BUILD_NUMBER}'
            }
        }
    }

    post {
        success {
            echo 'Docker image pushed successfully!'
        }

        failure {
            echo 'Pipeline failed. Check the console output.'
        }

        always {
            sh 'docker logout || true'
        }
    }
}
