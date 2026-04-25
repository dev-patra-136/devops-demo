pipeline {
    agent any

    environment {
        IMAGE_NAME = "devpatra136/my-html-app"
        KUBECONFIG_PATH = "C:\\Users\\Debasish\\.kube\\config"
    }

    stages {

        stage('Build Docker Image') {
            steps {
                bat 'docker build -t %IMAGE_NAME%:latest .'
            }
        }

        stage('Login & Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-creds', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                    bat '''
                    echo %PASS% | docker login -u %USER% --password-stdin
                    docker push %IMAGE_NAME%:latest
                    '''
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                bat '''
                set KUBECONFIG=%KUBECONFIG_PATH%
                kubectl delete deployment my-html --ignore-not-found
                kubectl create deployment my-html --image=%IMAGE_NAME%:latest
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
