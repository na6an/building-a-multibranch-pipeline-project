pipeline {
    agent {
        docker {
            image 'node:6-alpine'
            args '-p 3000:3000 -p 5000:5000'
        }
    }
    environment {
        CI = 'true'
    }
    stages {
        stage('Build') {
            steps {
                sh 'npm install'
            }
        }
        stage('Test') {
            steps {
                sh './jenkins/scripts/test.sh'
            }
        }
        stage('Deliver for dev') {
            when {
                branch 'dev' 
            }
            steps {
                sh './jenkins/scripts/deliver-for-development.sh'
                input message: 'Finished? (Click "Proceed")'
                sh './jenkins/scripts/kill.sh'
            }
        }
        stage('Deploy for prod') {
            when {
                branch 'prod'  
            }
            steps {
                sh './jenkins/scripts/deploy-for-production.sh'
                input message: 'Finished? (Click "Proceed")'
                sh './jenkins/scripts/kill.sh'
            }
        }
    }
}
