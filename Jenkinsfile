pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "flaskapp:1.0"
        SERVICE_NAME = "flask_service"
        SERVICE_PORT = "8080"
        CONTAINER_PORT = "5000"
    }

    stages {

        stage('Checkout Source Code') {
            steps {
                // Checkout from main branch
                git branch: 'main', url: 'https://github.com/Gowri0126/flask-app.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "docker build -t flaskapp:1.0 ."
            }
        }
        stage('Push Docker Image') {
    steps {
        # Tag for Docker Hu
        docker tag flaskapp:1.0 gowri032/flask-app:1.0

        # Login to Docker Hub (make sure credentials stored in Jenkins)
        echo "g123456789" | docker login -u "gowri032" --password-stdin

        # Push to Docker Hub
        docker push gowri032/flask-app:1.0
        """
    }
}

        stage('Deploy to Docker Swarm') {
            steps {
                sh """
                # Remove old service if exists
                docker service rm flask_service || true

                # Create new service
                docker service create \
                  --name flask_service \
                  --publish published=8080,target=5000 \
                  gowri032/flaskapp:1.0
                """
            }
        }
        
    }

    post {
        success {
            echo "Pipeline finished successfully. Flask app deployed on Docker Swarm!"
        }
        failure {
            echo "Pipeline failed. Check console output for errors."
        }
    }
}
