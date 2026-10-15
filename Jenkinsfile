pipeline{
    agent any

    environment {
        DOCKERHUB_CREDS = credentials('dockerhub-creds')   // creates _USR and _PSW variables
        IMAGE_NAME      = 'yourdockerhubuser/jenkins-demo-app'
        IMAGE_TAG       = "${BUILD_NUMBER}"
    }
    stages{
        stage('Clone repo'){
            steps{
                git branch: 'main', url: 'https://github.com/SARANRAJAk7010/Two_Tier_Python_Project.git'
            }
        }
        stage('Build image'){
            steps{
                sh 'docker build -t $IMAGE_NAME:$IMAGE_TAG -t $IMAGE_NAME:latest .'
            }
        }
        stage('Login to Docker Hub') {
            steps {
                sh 'echo $DOCKERHUB_CREDS_PSW | docker login -u $DOCKERHUB_CREDS_USR --password-stdin'
            }
        }
        stage('Push Image') {
            steps {
                sh 'docker push $IMAGE_NAME:$IMAGE_TAG'
                sh 'docker push $IMAGE_NAME:latest'
            }
        }
    }
    post {
        always {
            sh 'docker logout || true'
            sh 'docker image prune -f'
        }
    }
}
