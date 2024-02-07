pipeline {
    agent any
    
    environment {
        DOCKER_IMAGE_TAG = 'latest' 
        DOCKERFILE_PATH = 'webmails/Dockerfile'
        DOCKERHUB_REGISTRY = 'docker.io'
        DOCKERHUB_REPOSITORY = 'cdelambert/mailu'
    }
    
  stage('Login') {
                steps {
                         sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
                 
                }
          }

        
        stage('Build Docker Image') {
            steps {
                // Construction de l'image Docker
                script {
                    docker.build("${DOCKERHUB_REGISTRY}/${DOCKERHUB_REPOSITORY}:${DOCKER_IMAGE_TAG}", "${DOCKERFILE_PATH}")
                }
            }
        }
        
        stage('Push Docker Image') {
            steps {
                // Pousser l'image Docker vers le registre DockerHub
                script {
                    docker.withRegistry("${DOCKERHUB_REGISTRY}", "${DOCKERHUB_CREDENTIALS_USR}", "${DOCKERHUB_CREDENTIALS_PSW}") {
                        docker.image("${DOCKERHUB_REGISTRY}/${DOCKERHUB_REPOSITORY}:${DOCKER_IMAGE_TAG}").push()
                    }
                }
            }
        }
    }
}
