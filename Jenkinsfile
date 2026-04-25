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

        stage('Check Login') {
    steps {
        withCredentials([usernamePassword(credentialsId: 'docker-creds', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
            bat '''
            echo Logging in...
            echo %PASS% | docker login -u %USER% --password-stdin
            '''
        }
    }
}

        stage('Login & Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-creds', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                    bat '''
                    echo %PASS% | docker login -u %USER% --password-stdin
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
