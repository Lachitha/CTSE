pipeline {
    agent any




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

