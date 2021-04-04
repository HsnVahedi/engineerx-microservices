pipeline {
    agent any
    parameters {
        string(name: 'CLUSTER_NAME', defaultValue: 'engineerx')
        string(name: 'REGION', defaultValue: 'us-east-2')
    }
    environment {
        REGION = "${params.REGION}"
        CLUSTER_NAME = "${params.CLUSTER_NAME}"
    }
    stages {
        stage('Invoke Integration Test Pipeline') {
            steps {
                build job: 'integration-test', parameters: [
                    string(name: "BACKEND_VERSION", value: "${env.BUILD_ID}")
                    string(name: "FRONTEND_VERSION", value: "${env.FRONTEND}")
                ]
            }
        }
        stage('Invoke Setting latest tags') {
            steps {
                build job: 'backend-latest-tag', parameters: [
                    string(name: "BACKEND_VERSION", value: "${env.BUILD_ID}"),
                ]
                build job: 'frontend-latest-tag', parameters: [
                    string(name: "FRONTEND_VERSION", value: "${env.FRONTEND_VERSION}")
                ]
            }
        }
    }
}
