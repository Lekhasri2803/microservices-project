pipeline {
    agent any

    stages {

        stage('Deploy To Kubernetes') {
            steps {
                withKubeCredentials(kubectlCredentials: [[
                    caCertificate: '',
                    clusterName: 'EKS-10',
                    contextName: '',
                    credentialsId: 'k8-token',
                    namespace: 'webapps',
                    serverUrl: 'https://A26D8727EDB717AA063E728A647CA18D.gr7.ap-south-1.eks.amazonaws.com'
                ]]) {

                    sh '''
                    kubectl apply -f deployment-service.yml
                    '''
                }
            }
        }

        stage('Verify Deployment') {
            steps {
                withKubeCredentials(kubectlCredentials: [[
                    caCertificate: '',
                    clusterName: 'EKS-10',
                    contextName: '',
                    credentialsId: 'k8-token',
                    namespace: 'webapps',
                    serverUrl: 'https://A26D8727EDB717AA063E728A647CA18D.gr7.ap-south-1.eks.amazonaws.com'
                ]]) {

                    sh '''
                    kubectl get pods -n webapps
                    kubectl get svc -n webapps
                    kubectl get deployments -n webapps
                    '''
                }
            }
        }
    }
}
