pipeline {
    agent {
        docker {
            image 'node:18'
            args '-v /var/run/docker.sock:/var/run/docker.sock'
        }
    }

    stages {
        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t ci-cd-demo:latest .'
            }
        }

        stage('Run Container') {
            steps {
                sh '''
                docker rm -f demo || true
                docker run -d --name demo -p 3000:3000 ci-cd-demo:latest
                '''
            }
        }
    }
}
pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                // Build Docker image using host network to avoid npm network issues
                sh 'docker build --network host -t ci-cd-demo:latest .'
            }
        }

        stage('Run Docker Container') {
            steps {
                // Optional: run the container to verify the image
                sh 'docker run --rm ci-cd-demo:latest'
            }
        }
    }
}
