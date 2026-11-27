pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                echo 'Checking out DevOps project from GitHub'
            }
        }

        stage('Build') {
            steps {
                echo 'Building PHP and Nginx application'
            }
        }

        stage('Test') {
            steps {
                echo 'Running application checks'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deployment stage will be configured for Kubernetes'
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed. Check the console output.'
        }
    }
}