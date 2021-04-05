pipeline {
    agent any
    parameters {
        string(name: 'CLUSTER_NAME', defaultValue: 'engineerx')
        string(name: 'REGION', defaultValue: 'us-east-2')
        string(name: 'BACKEND_VERSION', defaultValue: 'latest')
        string(name: 'FRONTEND_VERSION', defaultValue: 'latest')
    }
    environment {
        REGION = "${params.REGION}"
        CLUSTER_NAME = "${params.CLUSTER_NAME}"
        BACKEND_VERSION = "${params.BACKEND_VERSION}"
        FRONTEND_VERSION = "${params.FRONTEND_VERSION}"
    }
    stages {
        stage('Invoke Integration Test Pipeline') {
            steps {
                build job: 'integration-test', parameters: [
                    string(name: "BACKEND_VERSION", value: "${env.BACKEND_VERSION}"),
                    string(name: "FRONTEND_VERSION", value: "${env.FRONTEND_VERSION}")
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
