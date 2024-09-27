pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Glal-6/Glalkubernetes.git'
            }
        }
        stage('Deploy Database') {
            steps {
                sh 'kubectl apply -f k8s/db-secret.yaml -n cicd' 
                sh 'kubectl apply -f k8s/db-data-pv.yaml -n cicd' 
                sh 'kubectl apply -f k8s/db-data-pvc.yaml -n cicd' 
                sh 'kubectl apply -f k8s/database_deployment.yaml -n cicd' 
                sh 'kubectl apply -f k8s/db_service.yaml -n cicd' 
            }
        }
        stage('Deploy Backend') {
            steps {
                sh 'kubectl apply -f k8s/backend_deployment.yaml -n cicd' 
                sh 'kubectl apply -f k8s/backend_service.yaml -n cicd'
            }
        }
        stage('Deploy Proxy') {
            steps {
                sh 'kubectl apply -f k8s/proxy_deployment.yaml -n cicd' 
                sh 'kubectl apply -f k8s/proxy_nodeport.yaml -n cicd' 
            }
        }
    }
    post {
        success {
            echo 'Deployment successful!'
        }
        failure {
            echo 'Deployment failed!'
        }
    }
}
