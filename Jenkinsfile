pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                // Run npm install inside a Docker container using host network
                sh 'docker run --rm --network host -v $PWD:/app -w /app node:18 npm install'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build --network host -t ci-cd-demo:latest .'
            }
        }

        stage('Run Container') {
            steps {
                sh 'docker run --rm ci-cd-demo:latest'
            }
        }
    }
}
