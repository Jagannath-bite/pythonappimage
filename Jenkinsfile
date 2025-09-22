pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhubcred')
        IMAGE_NAME = "yourdockerhubusername/pythonapp"
        IMAGE_TAG = "latest"
    }

    stages {
        stage('Build Docker Image') {
            steps {
                sh "docker build -t $IMAGE_NAME:$IMAGE_TAG ."
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
                sh "docker rm -f pythonapp-container || true"
                sh "docker run -d --name pythonapp-container -p 5000:5000 $IMAGE_NAME:$IMAGE_TAG"
            }
        }
    }

    post {
        always {
            echo "Pipeline finished."
        }
    }
}
