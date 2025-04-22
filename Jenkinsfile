pipeline {
    agent any

    environment {
        IMAGE = "itsraghav/demo-app"
        REGISTRY_CREDENTIAL = 'DockerCred'
    }

    stages {
        stage('Build Image') {
            steps {
                script {
                    docker.build("${IMAGE}")
                }
            }
        }

        stage('Push to DockerHub') {
            steps {
                script {
                    docker.withRegistry('', REGISTRY_CREDENTIAL) {
                        docker.image("${IMAGE}").push()
                    }
                }
            }
        }

        stage('Deploy Container') {
            steps {
                sh 'docker rm -f demo-app || true'
                sh 'docker run -d -p 3000:3000 --name demo-app ${IMAGE}'
            }
        }
    }

