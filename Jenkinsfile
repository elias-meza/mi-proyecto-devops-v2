pipeline {
    agent any
    stages {
        stage("pipeline node") {
            agent {
                docker { 
                    image 'node:20.11.1-alpine3.19' 
                    reuseNode true
                }
            }
            stages {
                stage('instalar dependencias') {
                    steps {
                        sh 'npm install'
                    }
                }
                stage('ejecutar test') {
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
        stage("code quality") {
            stages {
                stage("build & SonarQube analysis") {
                    agent { 
                        docker { 
                            image 'sonarsource/sonar-scanner-cli'
                            args  '--network="devops-infra_default"'
                            reuseNode true
                        }
                    }
                    steps {
                        withSonarQubeEnv('sonarqube') {
                            sh 'sonar-scanner'
                        }
                    }
                }
                stage("Quality Gate") {
                    steps {
                        timeout(time: 1, unit: 'HOURS') {
                            waitForQualityGate abortPipeline: true
                        }
                    }
                }
            }
        }

        stage('hacer build') {
            steps {
                sh 'docker build -t mi-proyecto-devops .'
            }
        }
    }
}