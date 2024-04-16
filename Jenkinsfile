pipeline {
    agent any
    tools {
        nodejs 'node' // Use the NodeJS installation name defined in Jenkins
    }
    environment {
        DOCKER_CREDENTIALS_ID = 'docker-cred'
        DOCKER_IMAGE_NAME = 'lachisenarath576259/nodemain:latest'
        DOCKERFILE_PATH = '/var/lib/jenkins/workspace/MicroService_MERN_main/Dockerfile' // Update Dockerfile path
    }
    stages { 
        stage('Node JS Build') {
            steps {
                sh 'npm install'
            }
        }
        stage('Build & Tag Docker Image') {
            steps {
                script {
                    sh 'export DOCKER_BUILDKIT=1'
                    sh "docker buildx build --tag $DOCKER_IMAGE_NAME --file $DOCKERFILE_PATH ."
                }
            }
        }
        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', DOCKER_CREDENTIALS_ID) {
                        docker.image(DOCKER_IMAGE_NAME).push('latest')
                    }
                }
            }
        }
        stage('Deploy To Kubernetes') {
            steps {
                withKubeConfig(credentialsId: 'k8-token', serverUrl: 'https://800F84F142BBCD88A8B3616C37B6CB94.gr7.ap-southeast-1.eks.amazonaws.com', namespace: 'webapps') {
                    sh "kubectl apply -f deployment-service.yml"
                }
            }
        }
        stage('Verify Deployment') {
            steps {
                withKubeConfig(credentialsId: 'k8-token', serverUrl: 'https://800F84F142BBCD88A8B3616C37B6CB94.gr7.ap-southeast-1.eks.amazonaws.com', namespace: 'webapps') {
                    sh "kubectl get svc -n webapps"
                }
            }
        }
    }
}
