pipeline {
    agent any

    environment {
        IMAGE_NAME = "jagannath239/pythonapp:latest"
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/Jagannath-bite/pythonappimage.git',
                    credentialsId: 'dockerhubcred'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME .'
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhubcred', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh '''
                      echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                      docker push $IMAGE_NAME
                    '''
                }
            }
        }

        stage('Run Container') {
            steps {
                sh '''
                  docker rm -f pythonapp || true
                  docker run -d --name pythonapp -p 5000:5000 $IMAGE_NAME
                '''
            }
        }
    }
}
