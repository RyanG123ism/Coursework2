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
                    // Run a temporary container to test the launch
                    def exitCode = sh(script: 'docker run --rm -d ryang123ism/myimage:1.0 /bin/sh -c "while true; do sleep 10; done"', returnStatus: true)

                    // Check if the container launched successfully
                    if (exitCode == 0) {
                        echo 'Container launch test passed!'
                    } else {
                        error 'Container launch test failed!'
                    }
                }
            }
        }

        stage('Push to DockerHub') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub-credentials') {
                        docker.image("ryang123ism/myimage:1.0").push()
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
        post {
        success {
            echo 'Pipeline succeeded!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}
