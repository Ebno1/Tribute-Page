pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub_id')
    }

    stages {
        stage('Checkout SCM') {
            steps {
                checkout scm
            }
        }
    
        stage('Build Docker image') {
            steps {
                sh 'docker build -t ebno1/tribute-page:latest .'
                }
        }

        stage('Login'){
            steps {
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
            }
        }

        stage('Push Docker image to Docker Hub') {
            steps {
                sh 'docker push ebno1/tribute-page:latest'
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                script {
                    kubernetesDeploy(configs: "deploymentservice.yaml")
                    // sh 'kubectl apply -f deployment.yaml'
                    // sh 'kubectl apply -f kubernetes/service.yaml'
                }
            }
        }

        stage('Deploy to K8s') {
            steps{
                script {
                sh "sed -i 's,TEST_IMAGE_NAME,harshmanvar/node-web-app:$BUILD_NUMBER,' deploymentservice.yaml"
                sh "cat deploymentservice.yaml"
                sh "kubectl --kubeconfig=/home/ec2-user/config get pods"
                sh "kubectl --kubeconfig=/home/ec2-user/config apply -f deploymentservice.yaml"
                }
            }
        }

    }
    post {
        always {
            sh 'docker logout'
        }
    }
}