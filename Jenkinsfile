pipeline {
    agent any
    environment {
        // Optional: If you want to use KUBECONFIG directly
        KUBECONFIG = credentials('kubeconfig-cred')
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'master', url: 'https://github.com/rangakrish7/helm-your-chart-.git'
            }
        }
        stage('Install Helm') {
            steps {
                sh '''
                curl https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 | bash
                helm version
                '''
            }
        }
        stage('Deploy with Helm') {
            steps {
                withKubeConfig([credentialsId: 'kubeconfig-cred']) {
                    sh '''
                    helm repo add jenkins https://charts.jenkins.io
                    helm repo update
                    helm upgrade --install jenkins jenkins/jenkins --namespace jenkins --create-namespace -f values.yaml
                    kubectl get pods -n jenkins
                    '''
                }
            }
        }
    }
}
