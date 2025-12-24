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
                    credentialsId: 'aws-creds-4eks',  // Jenkins credential ID with AWS keys
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
                withCredentials([[
                    $class: 'AmazonWebServicesCredentialsBinding',
                    credentialsId: 'aws-creds-4eks',
                    accessKeyVariable: 'AWS_ACCESS_KEY_ID',
                    secretKeyVariable: 'AWS_SECRET_ACCESS_KEY'
                ]]) {
                    sh """
                        kubectl get namespace ${K8S_NAMESPACE}
                    """
                }
            }
        }

    }

    post {
        success {
            emailext(
                subject: "✅ SUCCESS: ${JOB_NAME} #${BUILD_NUMBER}",
                mimeType: 'text/html',
                body: """
                    <h2 style="color:green;">Build Successful 🎉</h2>
                    <p><b>Job:</b> ${JOB_NAME}</p>
                    <p><b>Build Number:</b> ${BUILD_NUMBER}</p>
                    <p><b>Status:</b> SUCCESS</p>
                    <p>
                        <b>Build URL:</b>
                        <a href="${BUILD_URL}">${BUILD_URL}</a>
                    </p>
                """,
                to: "erdigvijaypatil01@gmail.com"
            )
        }

        failure {
            emailext(
                subject: "❌ FAILURE: ${JOB_NAME} #${BUILD_NUMBER}",
                mimeType: 'text/html',
                body: """
                    <h2 style="color:red;">Build Failed ⚠️</h2>
                    <p><b>Job:</b> ${JOB_NAME}</p>
                    <p><b>Build Number:</b> ${BUILD_NUMBER}</p>
                    <p><b>Status:</b> FAILURE</p>
                    <p>
                        <b>Build URL:</b>
                        <a href="${BUILD_URL}">${BUILD_URL}</a>
                    </p>
                """,
                to: "erdigvijaypatil01@gmail.com"
            )
        }
    }

}
