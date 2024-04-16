pipeline {
    agent any
     tools {
        nodejs 'node' // Use the NodeJS installation name defined in Jenkins
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
                    withDockerRegistry(credentialsId: 'docker-cred', toolName: 'docker') {
                        sh "docker build -t lachisenarath576259/nodemain:latest"
                    }
                }
            }
        }
        
        stage('Push Docker Image') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'docker-cred', toolName: 'docker') {
                        sh "docker push lachisenarath576259/nodemain:latest "
                    }
                }
            }
        }
        stage('Deploy To Kubernetes') {
            steps {
               withKubeCredentials(kubectlCredentials: [[caCertificate: '', clusterName: 'EKS-2', contextName: '', credentialsId: 'k8-token', namespace: 'webapps', serverUrl: 'https://800F84F142BBCD88A8B3616C37B6CB94.gr7.ap-southeast-1.eks.amazonaws.com']]) {
                 sh "kubectl apply -f deployment-service.yml"
                }  
            }
        }
         
           stage('verify Deployment') {
            steps {
               withKubeCredentials(kubectlCredentials: [[caCertificate: '', clusterName: 'EKS-2', contextName: '', credentialsId: 'k8-token', namespace: 'webapps', serverUrl: 'https://800F84F142BBCD88A8B3616C37B6CB94.gr7.ap-southeast-1.eks.amazonaws.com']]) {
                 sh "kubectl get svc -n webapps"
                }
            }
        }
    }
    
}
