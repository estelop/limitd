pipeline {
    agent any
    tools {
        nodejs 'node-v4.4.4'
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
                sh "export VERSION_NUMBER=0.0.0 && export WORKSPACE=. && make"
                sh "mv limitd_0.0.0_amd64.deb ${params.bundle_file}"
                archiveArtifacts "${params.bundle_file}"
            }
        }
    }
}