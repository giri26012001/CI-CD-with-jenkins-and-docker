pipeline {
    agent any 
    stages {
        stage('Build') {
            steps {
                script {
                    docker.build("myapp:${env.BUILD_NUMBER}")
                }
            }
        }
        stage('Test') {
            steps {
                sh 'echo "Running tests"'
            }
        }
    }
}
