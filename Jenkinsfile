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
                    def testContainer = docker.image('your-docker-image:latest').run('-d', '/bin/sh', '-c', 'while true; do sleep 10; done')

                    // Check the exit code to ensure the container launched successfully
                    def exitCode = testContainer.inside("/bin/sh -c 'exit 0'", returnStatus: true)
                    if (exitCode == 0) {
                        echo 'Container launch test passed!'
                    } else {
                        error 'Container launch test failed!'
                    }

                    // Stop and remove the temporary container
                    testContainer.stop()
                    testContainer.remove()
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
        post {
        success {
            echo 'Pipeline succeeded!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}
