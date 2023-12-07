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
                // Build the Docker image using the Dockerfile in the repository
                script {
                    app = docker.build("ryang123ism/myimage:1.0")
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    // Run tests inside the container
                    docker.image("ryang123ism/myimage:1.0").inside {
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
