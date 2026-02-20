pipeline {
    agent any

    stages {
        stage('Clone Repository') {
            steps {
                checkout scm
            }
        }

        stage('Build Backend Image') {
            steps {
                sh 'docker build -t backend-image ./backend'
            }
        }

        stage('Run Backend Container') {
            steps {
                sh 'docker run -d --name backend backend-image'
            }
        }

        stage('Run NGINX') {
            steps {
                sh 'docker run -d -p 80:80 --name nginx nginx'
            }
        }
    }
}
