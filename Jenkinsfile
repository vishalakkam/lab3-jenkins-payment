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
                    "%DOCKER%" tag %IMAGE%:%TAG% localhost:5000/%IMAGE%:%TAG%
                    "%DOCKER%" images
                '''
            }
        }

        stage('Push') {
            steps {
                bat '''
                    "%DOCKER%" push localhost:5000/%IMAGE%:%TAG%
                '''
            }
        }

        stage('Deploy') {
    steps {
        bat '''
            "%DOCKER%" stop payment >nul 2>&1 || echo No existing payment container
            "%DOCKER%" rm payment >nul 2>&1 || echo No existing payment container

            "%DOCKER%" run -d ^
              --name payment ^
              -p 8080:80 ^
              localhost:5000/%IMAGE%:%TAG%

            "%DOCKER%" ps --filter "name=payment"
        '''
    }
}
    }
}