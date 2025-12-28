pipeline {
    agent any

    environment {
        IMAGE_NAME = "ci-cd-demo"
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                echo "Installing npm dependencies using host network..."
                sh '''
                docker run --rm \
                  --network host \
                  -v "$PWD:/app" \
                  -w /app \
                  node:18 npm install
                '''
            }
        }

        stage('Build Docker Image') {
            steps {
                echo "Building Docker image..."
                sh '''
                docker build \
                  --network host \
                  -t $IMAGE_NAME:latest .
                '''
            }
        }

        stage('Run Container') {
            steps {
                echo "Running container in detached mode..."
                sh '''
                docker stop ci-cd-demo || true
                docker rm ci-cd-demo || true

                docker run -d \
                  --name ci-cd-demo \
                  -p 3000:3000 \
                  $IMAGE_NAME:latest
                '''
            }
        }
    }

    post {
        success {
            echo "✅ Pipeline completed successfully"
        }
        failure {
            echo "❌ Pipeline failed"
        }
        always {
            echo "🧹 Cleaning up workspace"
            cleanWs()
        }
    }
}
