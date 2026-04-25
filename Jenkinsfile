pipeline {
    agent any

    environment {
        DOCKERHUB_USER = "devpatra136"
        IMAGE_NAME = "my-html-app"
        FULL_IMAGE = "${DOCKERHUB_USER}/${IMAGE_NAME}:latest"
        KUBECONFIG_PATH = "C:\\Users\\Debasish\\.kube\\config"
    }

    stages {

        stage('Build Docker Image') {
            steps {
                bat "docker build -t %FULL_IMAGE% ."
            }
        }

        stage('Login & Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-creds', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                    bat '''
                    docker login -u %USER% -p %PASS%
                    docker push %FULL_IMAGE%
                    '''
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                bat '''
                set KUBECONFIG=%KUBECONFIG_PATH%

                kubectl delete deployment my-html --ignore-not-found
                kubectl create deployment my-html --image=%FULL_IMAGE%
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
