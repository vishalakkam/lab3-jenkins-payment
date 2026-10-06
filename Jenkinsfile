pipeline {
    agent any

    environment {
        IMAGE = "mycompany/payment"
        TAG = "${BUILD_NUMBER}"
        DOCKER = "C:\\Users\\DELL\\AppData\\Local\\Programs\\DockerDesktop\\resources\\bin\\docker.exe"
    }

    stages {
        stage('Build') {
            steps {
                bat '''
                    "%DOCKER%" build -t %IMAGE%:%TAG% .
                '''
            }
        }
    }
}