pipeline{
    agent any
    environment {
        DOCKER_HUB_USERNAME = credentials('docker-hub-credentials-username')
        IMAGE_NAME = 'my-app'
        IMAGE_TAG = "${BUILD_NUMBER}"
        CONTAINER_NAME = 'my-running-app'
    }
    stages{
        stage ('clone') {
            steps {
                git clone 'https://github.com/giri26012001/CI-CD-with-jenkins-and-docker.git'
            }
        }
        stage ('unit tests') {
            steps {
                echo 'running tests'
            }
        }
        stage ('build') {
            steps {
                sh "docker-build -t ${DOCKER_HUB_USERNAME}/${IMAGE_NAME}:${IMAGE_TAG} ."
                sh "docker tag ${DOCKER_HUB_USERNAME}/${IMAGE_NAME}:${DOCKER_HUB_USERNAME}/${IMAGE_NAME}:latest"
            }
        }
        stage ('push') {
            steps {
                withCredentials([usernamePassword(credentialsID: 'docker-hub-credentials', passwordVariable: 'DOCKER_PASS', usernameVariable: 'DOCKER_USER')]) {
                    sh "echo \$DOCKER_PASS | docker login -u \$DOCKER_USER --password-stdin"
                    sh "docker push ${DOCKER_HUB_USERNAME}/${IMAGE_NAME}:${IMAGE_TAG}"
                    sh "docker push ${DOCKER_HUB_USERNAME}/${IMAGE_NAME}:latest"
                }
            } 
        }
        stage ('deploy') {
            steps {
                echo 'deploying application'
                sh "docker stop ${COTAINER_NAME} || true"
                sh "docker rm ${CONTAINER_NAME} || true"
                sh "docker pull ${DOCKER_HUB_USERNAME}/${IMAGE_NAME}:latest"
                sh "docker run -d --name ${CONTAINER_NAME} -p 3000:3000 ${DOCKER_HUB_USERNAME}/${IMAGE_NAME}:latest"
            }
        }
    }
}