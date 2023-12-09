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
                    def imageTag = "${env.BUILD_NUMBER}"
                    
                    app = docker.build("ryang123ism/myimage:${imageTag}")
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    def imageTag = "${env.BUILD_NUMBER}"
                    // Run a temporary container to test the launch
                    def exitCode = sh(script: 'docker run --rm -d ryang123ism/myimage:' + imageTag + ' /bin/sh -c "while true; do sleep 10; done"', returnStatus: true)

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
                    def imageTag = "${env.BUILD_NUMBER}"
                    
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub-credentials') {
                        docker.image("ryang123ism/myimage:${imageTag}").push()
                    }
                }
            }
        }
        
        stage('Deploy to Kubernetes') {
            steps{                         
                script { 
                    def imageTag = "${env.BUILD_NUMBER}"
                    
                    withCredentials([sshUserPrivateKey(credentialsId: 'my-ssh-key', keyFileVariable: "KEY_FILE")]) {                                                
                        sh "ssh -o StrictHostKeyChecking=no -o Protocol=1 -i $KEY_FILE ubuntu@52.87.204.39 \"kubectl set image deployments/myapp-deployment-v2 myimage=ryang123ism/myimage:${imageTag}\""
                    }           
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
