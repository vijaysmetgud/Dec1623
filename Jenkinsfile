pipeline {
    agent { label 'docker' }
    stages {
        stage('clean workspace'){
            steps{
                cleanWs()
            }
        }
        stage('Checkout from Git'){
            steps{
                git branch: 'main', url: 'https://github.com/KhajaWorkshopsAtQT/Dec1623.git'
            }
        }
        stage('Build docker image') {
            steps {
                sh "docker image build -t shaikkhajaibrahim/jenkinsdec23workshop:$BUILD_ID ."
            }
        }
        stage('Trivy Scan') {
            steps {
                script {
                    sh "trivy image --format json -o trivy-report.json shaikkhajaibrahim/jenkinsdec23workshop:$BUILD_ID"
                }
                publishHTML([reportName: 'Trivy Vulnerability Report', reportDir: '.', reportFiles: 'trivy-report.json', keepAll: true, alwaysLinkToLastBuild: true, allowMissing: false])
            }
        }
        stage('publish docker image') {
            steps {
                sh "docker image push shaikkhajaibrahim/jenkinsdec23workshop:$BUILD_ID"
            }
        }
        stage('deploy to k8s') {
            steps {
                sh "kubectl apply -f deployment/k8s/deployment.yaml"
                sh """
                kubectl patch deployment netflix-app -p '{"spec":{"template":{"spec":{"containers":[{"name":"netflix-app","image":"shaikkhajaibrahim/jenkinsdec23workshop:$BUILD_ID"}]}}}}'
                """
            }
        }

        stage('kube-bench Scan') {
            steps {
                script {
                    sh "/home/ubuntu/kubebench/kube-bench --json > kube-bench-report.json"
                }
                publishHTML([reportName: 'kube-bench Report', reportDir: '.', reportFiles: 'kube-bench-report.json', keepAll: true, alwaysLinkToLastBuild: true, allowMissing: false])
            }
        }
    }
}