pipeline {
    agent any
    environment {
        KUBECONFIG = credentials('kubeconfig-cred') // Jenkins credential for kubeconfig
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
                sh '''
                helm repo add jenkins https://charts.jenkins.io
                helm repo update
                helm upgrade --install jenkins jenkins/jenkins --namespace jenkins --create-namespace -f values.yaml
                '''
            }
        }
    }
}
