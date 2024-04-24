pipeline {
    agent any
    stages {
        stage('node pipeline') {
            agent {
                docker { image 'node:20.11.1-alpine3.19' }
            }
            stages {
                stage('checkout') {
                    steps {
                        checkout scm
                    }

                }
                stage('instalar dependencias.') {
                    steps {
                        sh 'npm install'
                    }
                }
                stage('genear ejecutar test') {
                    steps {
                        sh 'npm run test'
                    }
                }
                stage('hacer build') {
                    steps {
                        sh 'npm run build'
                    }
                }
            }
        }
        stage('docker pipeline') {
            steps {
                sh 'docker build -t mi-proyecto-devops .'
            }
        }
    }
}