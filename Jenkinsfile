pipeline {
    agent any

    environment {
        IMAGE_NAME = "getting-started-app:latest"
        CONTAINER_NAME = "getting-started-app-container"
        DOCKERHUB_REPO = "guruteja934/get-started-app"
        DOCKER_TAG = "latest"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/Gur-Teja-Dadiboyina/getting-started-app.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "docker build -t ${IMAGE_NAME} ."
            }
        }

        stage('Run Container') {
            steps {
                sh """
                docker stop ${CONTAINER_NAME} || true
                docker rm ${CONTAINER_NAME} || true
                docker run -d -p 3030:5000 --name ${CONTAINER_NAME} ${IMAGE_NAME}
                """
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'Docker-Hub', usernameVariable: 'DOCKERHUB_USER', passwordVariable: 'DOCKERHUB_PASS')]) {
                    sh '''
                    echo "${DOCKERHUB_PASS}" | docker login -u "${DOCKERHUB_USER}" --password-stdin
                    docker tag getting-started-app:latest ${DOCKERHUB_USER}/get-started-app:latest
                    docker push ${DOCKERHUB_USER}/get-started-app:latest
                    docker logout
                    '''
                }
            }
        }
    }

    post {
        always {
            echo "Pipeline finished."
        }
    }
}
