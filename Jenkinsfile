pipeline {
    agent any

    stages {

        stage('Clone Source Code') {
            steps {
                git 'https://github.com/Gowri0126/flask-app.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t flaskapp:1.0 .'
            }
        }

        stage('Deploy to Docker Swarm') {
            steps {
                sh '''
                docker service rm flask_service || true

                docker service create \
                --name flask_service \
                --publish published=8080,target=5000 \
                flaskapp:1.0
                '''
            }
        }
    }
}
