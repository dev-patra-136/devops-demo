pipeline {
    agent any

    environment {
        IMAGE_NAME = "my-html-app"
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
        set KUBECONFIG=C:\\Users\\Debasish\\.kube\\config
        kubectl get nodes
        kubectl delete deployment my-html --ignore-not-found
        kubectl create deployment my-html --image=my-html-app
        '''
    }
}

        stage('Expose Service') {
            steps {
                bat '''
                kubectl delete svc my-html --ignore-not-found
                kubectl expose deployment my-html --type=NodePort --port=80
                '''
            }
        }
    }
}
