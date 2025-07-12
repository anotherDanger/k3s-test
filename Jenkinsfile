pipeline {
    agent any

    environment {
        FULLCHAIN = credentials('fullchain')
        PRIVKEY = credentials('privkey')
    }

    stages {
        stage('Create TLS Secret') {
            steps {
                sh '''
                mkdir -p tls
                cp "$FULLCHAIN" tls/tls.crt
                cp "$PRIVKEY" tls/tls.key
                kubectl create secret tls k8s4life --cert=tls/tls.crt --key=tls/tls.key --namespace=https --dry-run=client -o yaml | kubectl apply -f -
                '''
            }
        }
    }
}
