stage('Deploy Backends') {
    steps {
        sh '''
        docker network rm mynet || true
        docker network create mynet

        docker rm -f backend1 backend2 || true

        docker run -d --name backend1 --network mynet backend-app
        docker run -d --name backend2 --network mynet backend-app

        sleep 5
        '''
    }
}

stage('Setup NGINX') {
    steps {
        sh '''
        docker rm -f nginx-lb || true

        docker run -d --name nginx-lb -p 80:80 --network mynet nginx

        sleep 3

        docker cp nginx/default.conf nginx-lb:/etc/nginx/conf.d/default.conf

        docker exec nginx-lb nginx -s reload
        '''
    }
}
