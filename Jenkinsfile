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
                script {
                    docker.build("your-dockerhub-username/your-image-name")
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    // Run tests inside the container
                    docker.image("your-dockerhub-username/your-image-name").inside {
                        sh 'npm test'
                    }
                }
            }
        }

        stage('Push to DockerHub') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub-credentials') {
                        docker.image("your-dockerhub-username/your-image-name").push()
                    }
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                script {
                    // Use kubectl to apply Kubernetes manifests
                    sh 'kubectl apply -f your-kubernetes-manifest.yaml'
                }
            }
        }
    }
}
