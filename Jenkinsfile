pipeline {
    agent any

    environment {
        K8S_NAMESPACE = "automotive"
        AWS_DEFAULT_REGION = "us-east-1"
    }

    stages {

        stage('Create Namespace') {
            steps {
                withCredentials([[
                    $class: 'AmazonWebServicesCredentialsBinding',
                    credentialsId: 'aws-creds-4eks',  // Update ID that we used in jenkins credentials
                    accessKeyVariable: 'AWS_ACCESS_KEY_ID',
                    secretKeyVariable: 'AWS_SECRET_ACCESS_KEY'
                ]]) {
                    sh """
                        aws eks update-kubeconfig --name automotive-cluster --region $AWS_DEFAULT_REGION
                        kubectl apply -f namespace.yaml
                    """
                }
            }
        }

        stage('Verify Namespace') {
            steps {
                sh """
                    kubectl get namespace ${K8S_NAMESPACE}
                """
            }
        }
    }

    post {
        success {
            echo "✅ Namespace '${K8S_NAMESPACE}' is ready"
        }
        failure {
            echo "❌ Namespace creation failed"
        }
    }
}
