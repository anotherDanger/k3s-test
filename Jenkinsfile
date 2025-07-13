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
        stage('Checkout scm'){
            steps {
                checkout scm
            }
        }
        stage('Cleanup'){
            steps{
                sh 'kubectl delete -f ingress.yaml -n https'
            }
        }
        stage('Create TLS Secret'){
            steps{
                withCredentials([
                    file(credentialsId: 'fullchain', variable: 'FULLCHAIN'),
                    file(credentialsId: 'privkey', variable: 'PRIVKEY')
                ]){
                    sh '''
                        kubectl create namespace https --dry-run=client -o yaml | kubectl apply -f -
                        kubectl create secret tls k8s4life \
                            --cert=$FULLCHAIN \
                            --key=$PRIVKEY \
                            -n https --dry-run=client -o yaml | kubectl apply -f -
                    '''
                }
            }
        }
        stage('Deploy nginx'){
            steps{
                sh 'kubectl apply -f ingress.yaml -n https'
            }
        }
    }
}
