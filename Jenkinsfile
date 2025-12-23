pipeline {
    agent any

    environment {
        K8S_NAMESPACE = "automotive"
    }

    stages {

        stage('Create Namespace') {
            steps {
                sh """
                  kubectl apply -f namespace.yaml
                """
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
