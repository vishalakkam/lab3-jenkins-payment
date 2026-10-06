pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                bat '''
                    where docker
                    docker --version
                '''
            }
        }
    }
}