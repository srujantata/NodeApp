pipeline {
    agent any

    stages {
        stage('Pre Cleanup Workspace') {
            steps {
                script {
                    sh label: '', script: 'docker system prune -f > /dev/null 2>&1'
                    cleanWs()
                }
            }
        }
        stage('Checkout SCM') {
            steps {
                script {
                    git credentialsId: 'githubid', url: 'https://github.com/srujantata/NodeApp.git'
                }
            }
        }
        stage('NPM Install') {
            steps {
                script {
                    sh label: '', script: 'npm install'
                }
            }
        }
        stage('Docker Build') {
            steps {
                script {
                   sh label: '', script: 'docker build -t srujan .'
                }
            }
        }
        stage('Docker Push to ECR') {
            steps {
                script {
                    docker.withRegistry('https://550323674769.dkr.ecr.us-west-2.amazonaws.com/srujan:latest', 'ecr:us-west-2:AWSCredentials') {
                        docker.image('srujan').push('latest')
                    }
                }
            }
        }
        stage('Post Cleanup Workspace') {
            steps {
                script {
                    sh label: '', script: 'docker system prune -f > /dev/null 2>&1'
                    cleanWs()
                }
            }
        }
    }
}