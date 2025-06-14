pipeline {
    agent my-app

    environment {
        DOCKER_IMAGE = "samratsooraj/forkflyss:latest"
    }

    stages {
        stage("Clone Code") {
            steps {
                echo "Cloning the project..."
                git branch: 'master', url: 'https://github.com/Samrat-Suraj/Docker_Compose_Project.git'
            }
        }

        stage("Build Image") {
            steps {
                echo "Building Docker image..."
                sh "docker build -t ${DOCKER_IMAGE} ."
            }
        }

        stage("Push Image to Docker Hub") {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerHub',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh '''
                        echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                        docker push $DOCKER_IMAGE
                    '''
                }
            }
        }

        stage("Deploy with Docker Compose") {
            steps {
                echo "Deploying application with Docker Compose..."
                sh "docker compose down"
                sh "docker compose up --build -d"
            }
        }
    }
}
