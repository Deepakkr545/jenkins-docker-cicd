pipeline {
    agent any

    environment {
        IMAGE_NAME = "jenkins-docker-app"
        DOCKER_HUB_REPO = "your-docker-hub-username/jenkins-docker-app"
    }

    stages {
        stage('Checkout'){
            steps {
                git url: 'https://github.com/Deepakkr545/jenkins-docker-cicd.git'
            }
        }

        stage('Build Docker Image'){
            steps {
                echo ' Builing Docker Image...'
                sh 'docker build -t $IMAGE_NAME .'
            }
        }

        stage('Testing'){
            steps {
                echo 'Running Tests...'
                 sh 'pip install -r requirements.txt pytest'
                 sh 'pytest test_app.py'
            }
        }

        stage('Deploy to Docker Hub'){
            steps {
                echo 'Pushing Docker Image to Docker Hub...'
                withCredentials([usernamePassword(credentialsId: 'docker-hub-creds', usernameVariable: 'DOCKER_HUB_USERNAME', passwordVariable: 'DOCKER_HUB_PASSWORD')]) {
                    sh "echo $DOCKER_HUB_PASSWORD | docker login -u $DOCKER_HUB_USERNAME --password-stdin"
                    sh "docker tag $IMAGE_NAME $DOCKER_HUB_REPO:latest"
                    sh "docker push $DOCKER_HUB_REPO:latest"
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}
