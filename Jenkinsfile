pipeline {
    agent any

    stages {
        stage('Build Image') {
            steps {
                bat 'docker build -t my-html-app .'
            }
        }

        stage('Run Container') {
            steps {
                bat 'docker stop my-container || exit 0'
                bat 'docker rm my-container || exit 0'
                bat 'docker run -d -p 8082:80 --name my-container my-html-app'
            }
        }
    }
}
