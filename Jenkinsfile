
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
                    echo Jenkins Build: %BUILD_NUMBER%
                    echo Git Commit: %GIT_COMMIT%
                    echo Branch: main

                    "%DOCKER%" build ^
                      --label "jenkins.build=%BUILD_NUMBER%" ^
                      --label "git.commit=%GIT_COMMIT%" ^
                      --label "git.branch=main" ^
                      -t %IMAGE%:%TAG% .
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

                    "%DOCKER%" tag %IMAGE%:%TAG% localhost:5000/mycompany/payment:%TAG%

                    "%DOCKER%" images
                '''
            }
        }

        stage('Push') {
            steps {
                bat '''
                    "%DOCKER%" push localhost:5000/mycompany/payment:%TAG%
                '''
            }
        }

        stage('Deploy') {
            steps {
                bat '''
                    echo ========================================
                    echo Application Version: %TAG%
                    echo Git Commit: %GIT_COMMIT%
                    echo Docker Image: %IMAGE%:%TAG%
                    echo Jenkins Build: %BUILD_NUMBER%
                    echo Branch: main
                    echo ========================================

                    "%DOCKER%" stop payment >nul 2>&1 || echo No existing payment container

                    "%DOCKER%" rm payment >nul 2>&1 || echo No existing payment container

                    "%DOCKER%" run -d ^
                      --name payment ^
                      -p 8080:80 ^
                      localhost:5000/mycompany/payment:%TAG%

                    "%DOCKER%" ps --filter "name=payment"
                '''
            }
        }
    }
}
```
