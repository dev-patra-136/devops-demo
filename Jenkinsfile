pipeline {
    agent any

    environment {
        IMAGE_NAME = "my-html-app"
        KUBECONFIG_PATH = "C:\\Users\\Debasish\\.kube\\config"
    }

    stages {

        stage('Build Docker Image') {
            steps {
                bat 'docker build -t %IMAGE_NAME% .'
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                bat '''
                set KUBECONFIG=%KUBECONFIG_PATH%
                kubectl get nodes
                kubectl delete deployment my-html --ignore-not-found
                kubectl create deployment my-html --image=%IMAGE_NAME%
                '''
            }
        }

        stage('Expose Service') {
            steps {
                bat '''
                set KUBECONFIG=%KUBECONFIG_PATH%
                kubectl delete svc my-html --ignore-not-found
                kubectl expose deployment my-html --type=NodePort --port=80
                '''
            }
        }
    }
}
