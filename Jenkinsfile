pipeline {
    agent any

    stages {
        stage('Deploy To Kubernetes') {
            steps {
                script {
                    withKubeCredentials(
                        credentialsId: 'k8-token',
                        serverUrl: 'https://800F84F142BBCD88A8B3616C37B6CB94.gr7.ap-southeast-1.eks.amazonaws.com',
                        caCertificate: '',
                        clusterName: 'EKS-2',
                        namespace: 'webapps',
                        contextName: ''
                    ) {
                        sh "kubectl apply -f deployment-service.yml"
                    }
                }
            }
        }

        stage('Verify Deployment') {
            steps {
                script {
                    withKubeCredentials(
                        credentialsId: 'k8-token',
                        serverUrl: 'https://800F84F142BBCD88A8B3616C37B6CB94.gr7.ap-southeast-1.eks.amazonaws.com',
                        caCertificate: '',
                        clusterName: 'EKS-2',
                        namespace: 'webapps',
                        contextName: ''
                    ) {
                        sh "kubectl get svc -n webapps"
                    }
                }
            }
        }
    }
}
