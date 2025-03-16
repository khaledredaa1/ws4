pipeline {
    agent any

    environment {
        KUBECONFIG = "/etc/kubernetes/admin.conf"
    }

    stages {
        stage('Clone Repository') {
            steps {
                git 'https://github.com/your-repo/vprofile.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker build -t app_from_jenkins:v4 .'
                }
            }
        }

        stage('Test Docker Image') {
            steps {
                script {
                    sh 'docker run -d --name lab4 -p 7070:8080 app_from_jenkins:v4'
                }
            }
        }

        stage('Remove Test Container') {
            steps {
                script {
                    sh 'docker rm -f lab4'
                }
            }
        }

        stage('Push Image to Registry') {
            steps {
                script {
                    sh 'docker tag app_from_jenkins:v4 your-docker-repo/app_from_jenkins:v4'
                    sh 'docker push your-docker-repo/app_from_jenkins:v4'
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                script {
                    // Ensure kubectl is available
                    sh 'kubectl version --client'

                    // Apply Kubernetes manifests
                    sh 'kubectl apply -f kubernetes/'
                }
            }
        }

        stage('Verify Deployment') {
            steps {
                script {
                    sh 'kubectl get pods -o wide'
                }
            }
        }
    }

    post {
        success {
            echo 'Deployment successful!'
        }
        failure {
            echo 'Deployment failed. Check logs for details.'
        }
    }
}
