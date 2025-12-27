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
