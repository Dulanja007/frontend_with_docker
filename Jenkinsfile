pipeline {
    agent any 
    
    stages { 
        stage('SCM Checkout') {
            steps {
                retry(3) {
                    git branch: 'main', url: 'https://github.com/Dulanja007/frontend_with_docker.git'
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

// pipeline {
//     agent {
//         docker {
//             image 'node:18' // Use a Node.js Docker image
//             args '-u root'  // Run as root to install dependencies
//         }
//     }
//     environment {
//         DOCKER_IMAGE = 'docker-frontend-final:v1' // Update with your desired image name
//     }
//     stages {
//         stage('Checkout') {
//             steps {
//                 echo 'Cloning repository...'
//                 git branch: 'main', url: 'https://github.com/Dulanja007/frontend_with_docker.git' // Update with your repo URL
//             }
//         }
//         stage('Install Dependencies') {
//             steps {
//                 echo 'Installing dependencies...'
//                 sh 'npm install'
//             }
//         }
//         stage('Run Tests') {
//             steps {
//                 echo 'Running tests...'
//                 sh 'npm test -- --watchAll=false'
//             }
//         }
//         stage('Build') {
//             steps {
//                 echo 'Building the React app...'
//                 sh 'npm run build'
//             }
//         }
//         stage('Docker Build') {
//             steps {
//                 echo 'Building Docker image...'
//                 sh 'docker build -t $DOCKER_IMAGE .'
//             }
//         }
//         stage('Docker Push') {
//             steps {
//                 echo 'Pushing Docker image...'
//                 withCredentials([string(credentialsId: 'docker-hub-password', variable: 'DOCKER_PASSWORD')]) {
//                     sh '''
//                         echo "$DOCKER_PASSWORD" | docker login -u your-dockerhub-username --password-stdin
//                         docker tag $DOCKER_IMAGE your-dockerhub-username/$DOCKER_IMAGE
//                         docker push your-dockerhub-username/$DOCKER_IMAGE
//                     '''
//                 }
//             }
//         }
//     }
//     post {
//         always {
//             echo 'Cleaning up workspace...'
//             cleanWs() // Cleans the workspace after the pipeline
//         }
//         success {
//             echo 'Pipeline completed successfully.'
//         }
//         failure {
//             echo 'Pipeline failed.'
//         }
//     }
// }



