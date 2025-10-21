pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('docker-hub') 
        IMAGE_NAME = "dark093/two-autentication"
        BUILD_VERSION = "1.0.${env.BUILD_ID}"
    }

    stages {
        stage('Checkout') {
            steps {
                echo "🔄 Clonando repositorio..."
                git branch: 'main', url: 'https://github.com/DaR3kDev/two-autentication.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    echo "🏗️ Construyendo imagen Docker..."
                    sh """
                        docker build -f Dockerfile.node \
                        --build-arg BUILD_VERSION=${BUILD_VERSION} \
                        -t ${IMAGE_NAME}:${BUILD_VERSION} .
                    """
                }
            }
        }

        stage('Login to DockerHub') {
            steps {
                echo "🔐 Iniciando sesión en DockerHub..."
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
            }
        }

        stage('Push to DockerHub') {
            steps {
                echo "🚀 Subiendo imagen a DockerHub..."
                sh """
                    docker push ${IMAGE_NAME}:${BUILD_VERSION}
                    docker tag ${IMAGE_NAME}:${BUILD_VERSION} ${IMAGE_NAME}:latest
                    docker push ${IMAGE_NAME}:latest
                """
            }
        }
    }

    post {
        always {
            echo "🧹 Limpieza final..."
            sh 'docker system prune -f || true'
        }
        success {
            echo "✅ Pipeline completado con éxito."
        }
        failure {
            echo "❌ El pipeline falló."
        }
    }
}