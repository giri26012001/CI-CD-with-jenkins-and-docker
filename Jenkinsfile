pipeline{
    agent any
    // tools {
    //     dockerTool 'docker'
    // }
    environment {
        DOCKER_HUB_USERNAME = credentials('docker-hub-credentials')
        IMAGE_NAME = 'my-app'
        IMAGE_TAG = "${BUILD_NUMBER}"
        CONTAINER_NAME = 'my-running-app'
    }
    stages{
        stage ('clone') {
            steps {
                checkout scm
                // git clone 'https://github.com/giri26012001/CI-CD-with-jenkins-and-docker.git'
            }
        }
        stage ('unit tests') {
            steps {
                echo 'running tests'
            }
        }
        stage ('build') {
            steps {
                sh 'docker build -t \${DOCKER_HUB_USERNAME_USR}/${IMAGE_NAME}:${IMAGE_TAG} .'
                sh 'docker tag \${DOCKER_HUB_USERNAME_USR}/${IMAGE_NAME}:${IMAGE_TAG} \${DOCKER_HUB_USERNAME_USR}/${IMAGE_NAME}:latest'
            }
        }
        stage ('push') {
            steps {
                sh 'echo "$DOCKER_HUB_USERNAME_PSW" | docker login -u "$DOCKER_HUB_USERNAME_USR" --password-stdin'
                sh 'docker push \${DOCKER_HUB_USERNAME_USR}/${IMAGE_NAME}:${IMAGE_TAG}'
                sh 'docker push \${DOCKER_HUB_USERNAME_USR}/${IMAGE_NAME}:latest'
            } 
        }
        stage ('deploy') {
            steps {
                echo 'deploying application'
                sh "docker stop ${CONTAINER_NAME} || true"
                sh "docker rm ${CONTAINER_NAME} || true"
                sh "docker pull ${DOCKER_HUB_USERNAME_USR}/${IMAGE_NAME}:latest"
                sh "docker run -d --name ${CONTAINER_NAME} --network host ${DOCKER_HUB_USERNAME_USR}/${IMAGE_NAME}:latest"            }
        }
    }
}