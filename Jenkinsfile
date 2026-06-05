pipeline{
    agent any
    environment {
        DOCKER_USER = credentials('docker-username')
        DOCKER_TOKEN = credentials('docker-token')
    }
    stages{
        stage('1.Build the container'){
            steps{
                echo 'Building backend image from Dockerfile'
                bat 'cd backend && docker build -t practical-backend:latest .'
            }
        }

        stage('2.Tagging and Pushing the image'){
            steps{
                echo 'Tagging the container'
                bat 'docker login -u ${DOCKER_USER} -p ${DOCKER_TOKEN}'
                bat 'docker tag practical-backend:latest ${DOCKER_USER}/practical-backend:latest'
                bat 'docker push ${DOCKER_USER}/practical-backend:latest'
            }
        }

        stage('3.Deploying the application'){
            steps{
                echo 'Deploying the application'
                bat 'docker run -d -p 3000:3000 practical-backend:latest'
            }
        }
    }

    post{
        success{
            echo 'Deployment successful'
        }
        failure{
            echo 'Deployment failed'
        }
    }
}