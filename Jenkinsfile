pipeline {
    agent any

    environment {
        IMAGE = "mycompany/payment"
        TAG = "${BUILD_NUMBER}"
    }

    stage('Build') {
    steps {
        bat '''
            where docker
            docker --version
        '''
    }
}

        stage('Test') {
            steps {
                bat '''
                    docker run $IMAGE:$TAG pytest
                '''
            }
        }

        stage('Deploy') {
            steps {
             bat '''
                    docker stop payment || true
                    docker rm payment || true

                    docker run -d \
                      --name payment \
                      -p 8080:8080 \
                      $IMAGE:latest
                '''
            }
        }
    }
}
