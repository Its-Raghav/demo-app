pipeline {
    agent any

    environment {
        IMAGE = "itsraghav/demo-app"
        REGISTRY_CREDENTIAL = 'DockerCred'
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/Its-Raghav/demo-app.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${IMAGE}")
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    docker.withRegistry('', REGISTRY_CREDENTIAL) {
                        docker.image("${IMAGE}").push()
                    }
                }
            }
        }

        stage('Deploy with Docker Compose') {
            steps {
                sh '''
                    docker stack rm demo-stack || true
                    sleep 5
                    docker stack deploy -c docker-compose.yml demo-stack
                '''
            }
        }
    }
}
