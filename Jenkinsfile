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
        stage('Create TLS Secret'){
            steps{
                withCredentials([
                    file(credentialsId: 'fuuchain', variable: 'FULLCHAIN'),
                    file(credentialsId: 'privkey', variable: 'PRIVKEY')
                ]){
                    sh '''
                        kubectl create secret tls k8s4life \
                            --cert=$FULLCHAIN \
                            --key=$PRIVKEY \
                            -n https --dry-run=client -o yaml | kubectl apply -f -
                    '''
                }
            }
        }
    }
}
