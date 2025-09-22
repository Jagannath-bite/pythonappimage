pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub') // ID from saved credentials
        IMAGE_NAME = "yourdockerhubusername/myapp"
        IMAGE_TAG = "latest"
    }

    stages {
        stage('Clone Repo') {
            steps {
                // For demo, we assume the code is on Jenkins host
                sh 'mkdir -p workspace && cp -r myapp/* workspace/'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "docker build -t $IMAGE_NAME:$IMAGE_TAG workspace/"
            }
        }

        stage('Push to Docker Hub') {
            steps {
                sh """
                    echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin
                    docker push $IMAGE_NAME:$IMAGE_TAG
                """
            }
        }

        stage('Run Container') {
            steps {
                // Stop existing container if exists
                sh "docker rm -f myapp-container || true"
                // Run new container
                sh "docker run -d --name myapp-container -p 4000:3000 $IMAGE_NAME:$IMAGE_TAG"
            }
        }
    }

    post {
        always {
            echo 'Pipeline finished.'
        }
    }
}
