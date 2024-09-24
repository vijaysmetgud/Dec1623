pipeline {
    agent any
    stages {
        stage('clean workspace'){
            steps{
                cleanWs()
            }
        }
        stage('Checkout from Git'){
            steps{
                git branch: 'main', url: 'https://github.com/vijaysmetgud/Dec1623.git'
            }
        }
        stage('Build docker image') {
            steps {
                sh "docker image build -t vsmetgud/jenkinsworkshop:$BUILD_ID ."
            }
        }
        stage('Trivy Scan') {
            steps {
                script {
                    sh "trivy image --format json -o trivy-report.json vsmetgud/jenkinsworkshop:$BUILD_ID"
                }
                publishHTML([reportName: 'Trivy Vulnerability Report', reportDir: '.', reportFiles: 'trivy-report.json', keepAll: true, alwaysLinkToLastBuild: true, allowMissing: false])
            }
        }
        stage('publish docker image') {
            steps {         
                sh "docker push vsmetgud/jenkinsworkshop:$BUILD_ID"
            }
        }
                                  
    }
}
