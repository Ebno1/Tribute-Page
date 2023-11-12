pipeline {
    agent any

    stages {
        stage('Checkout SCM') {
            steps {
                checkout scm
            }
        }
    
    stage('Build Docker image') {
        steps {
            docker.withRegistry('https://hub.docker.com', credentialsId: 'dockerHubCredentialsId') {
                dockerImage = docker.image('tribute-page:latest').build()
            }
        }
    }

    stage('Push Docker image to Docker Hub') {
        steps {
            docker.withRegistry('https://hub.docker.com', credentialsId: 'dockerHubCredentialsId') {
                sh "docker push ${dockerImage.fullName}"
            }
        }
    }

    //     stage('Run tests') {
    //         steps {
    //             sh 'npm test'
    //         }
    //     }

    //     stage('Build Docker image') {
    //         steps {
    //             docker build -t my-project-name:latest .
    //         }
    //     }

    //     stage('Stage application') {
    //         steps {
    //             withKubeConfig(kubeconfig: 'config') {
    //                 kubectl apply -f staging-deployment.yaml
    //             }
    //         }
    //     }

    //     stage('Deploy to production') {
    //         steps {
    //             withKubeConfig(kubeconfig: 'config') {
    //                 kubectl apply -f production-deployment.yaml
    //             }
    //         }
    //     }
    }
}