pipeline {
    agent any

    stages {

        stage('Clone Repo') {
            steps {
                git branch: 'main',
                url: 'https://github.com/<your-username>/CC_LAB-6.git'
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
                docker rm -f backend1 backend2 || true

                docker run -d --name backend1 backend-app
                docker run -d --name backend2 backend-app

                sleep 3
                '''
            }
        }

        stage('Run NGINX') {
            steps {
                sh '''
                docker rm -f nginx-lb || true

                docker run -d                   --name nginx-lb                   --link backend1                   --link backend2                   -p 80:80                   nginx

                sleep 2

                docker cp nginx/default.conf nginx-lb:/etc/nginx/conf.d/default.conf
                docker exec nginx-lb nginx -s reload
                '''
            }
        }
    }
}
