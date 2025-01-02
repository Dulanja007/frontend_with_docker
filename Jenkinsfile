pipeline {
    agent any 
    
    stages { 
        stage('SCM Checkout') {
            steps {
                retry(3) {
                    git branch: 'main', url: 'https://github.com/Dulanja007/frontend_with_docker'
                }
            }
        }
        stage('Build Docker Image') {
            steps {  
                bat 'docker build -t dulanja007/frontend-cicd_test:%BUILD_NUMBER% .'
            }
        }
        stage('Login to Docker Hub') {
            steps {
                withCredentials([string(credentialsId: 'cicddocker-password', variable: 'cicddocker-password')]) {
                    script {
                        bat 'docker login -u dulanja007 -p ${cicddocker-password}'
                    }
                }
            }
        }
        stage('Push Image') {
            steps {
                bat 'docker push dulanja007/frontend-cicd_test:%BUILD_NUMBER%'
            }
        }
    }
    post {
        always {
            bat 'docker logout'
        }
    }
}
