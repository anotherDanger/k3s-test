pipeline {
    agent any

    environment {
        KUBECONFIG = '/var/lib/jenkins/.kube/config'
    }

    stages {
        stage('Check Cluster') {
            steps {
                sh 'kubectl get nodes'
            }
        }
    }
}
