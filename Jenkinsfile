pipeline {
    agent any
    stages {
        stage('Checkout SCM') {
            steps {
                checkout scm
            }
        }
        stage('Build Backend Image') {
            steps {
                sh 'docker build -t backend-app ./backend'
            }
        }
        stage('Deploy Backend Containers') {
            steps {
                sh '''
                docker network create mynet || true
                docker rm -f backend1 backend2 || true
                docker run -d --name backend1 --network mynet backend-app
                docker run -d --name backend2 --network mynet backend-app
                sleep 5
                '''
            }
        }
        stage('Deploy NGINX Load Balancer') {
            steps {
                sh '''
                docker rm -f nginx-lb || true
                docker run -d --name nginx-lb \
                    --network mynet \
                    -p 80:80 \
                    -v $(pwd)/nginx/default.conf:/etc/nginx/conf.d/default.conf \
                    nginx
                '''
            }
        }
    }
    post {
        success {
            echo 'Pipeline executed successfully. NGINX load balancer is running.'
        }
        failure {
            echo 'Pipeline failed. Check console logs for errors.'
        }
    }
}
