pipeline {
    agent any
    tools {
        nodejs 'node-v6.10.2'
    }
    stages {
        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }
        stage('Test') {
            steps {
                sh 'npm test'
            }
        }
        stage('Bundle') {
            steps {
                zip zipFile: "bundle.zip", archive: true
            }
        }
    }
}