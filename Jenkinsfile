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

        stage('Test') {
            steps {
                bat '''
                    "%DOCKER%" run --name payment-test -d %IMAGE%:%TAG%
                    "%DOCKER%" ps --filter "name=payment-test"
                    "%DOCKER%" stop payment-test
                    "%DOCKER%" rm payment-test
                '''
            }
        }

        stage('Tag') {
            steps {
                bat '''
                    "%DOCKER%" tag %IMAGE%:%TAG% %IMAGE%:build-%TAG%
                    "%DOCKER%" images %IMAGE%
                '''
            }
        }
    }
}