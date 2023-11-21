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
                    kubernetesDeploy(configs: "deployment.yaml")
                    // sh 'kubectl apply -f deployment.yaml'
                    // sh 'kubectl apply -f kubernetes/service.yaml'
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