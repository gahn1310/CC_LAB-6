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
                sh 'docker build -t backend-app ./backend'
            }
        }

        stage('Deploy Backends') {
            steps {
                sh '''
                docker network create mynet || true

                docker rm -f backend1 backend2 || true

                docker run -d --name backend1 --network mynet backend-app
                docker run -d --name backend2 --network mynet backend-app

                # wait for containers to be ready
                sleep 5
                '''
            }
        }

        stage('Setup NGINX') {
            steps {
                sh '''
                docker rm -f nginx-lb || true

                docker run -d --name nginx-lb \
                --network mynet \
                -p 80:80 nginx

                # wait for nginx + network DNS
                sleep 5

                docker cp nginx/default.conf nginx-lb:/etc/nginx/conf.d/default.conf

                docker exec nginx-lb nginx -s reload
                '''
            }
        }
    }
}
