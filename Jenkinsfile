pipeline {
    agent any
    environment {
        DOCKER_CREDS = credentials('docker-credentials')
    }

    stages {
        stage('Build') {
            agent {
                docker {
                    image 'maven:3.6.3-openjdk-11-slim'
                    label 'docker-agent' 
                }
            }
            steps {
                sh 'mvn clean install'
            }
        }

        stage('SonarQube') {
            steps {
                script {
                    def scannerHome = tool 'scanner-default'
                    withSonarQubeEnv('sonar-server') {
                        sh "${scannerHome}/bin/sonar-scanner " +
                           "-Dsonar.projectKey=labgradle01 " +
                           "-Dsonar.projectName=labgradle01 " +
                           "-Dsonar.sources=src/main/kotlin " +
                           "-Dsonar.java.binaries=build/classes " +
                           "-Dsonar.tests=src/test/kotlin"
                    }
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker --version'
                    sh 'docker build -t labgradle01:latest .'
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    sh 'docker login -u ${DOCKER_CREDS_USR} -p ${DOCKER_CREDS_PSW}'
                    sh 'docker tag msmicroservice ${DOCKER_CREDS_USR}/msmicroservice:latest'
                    sh 'docker push ${DOCKER_CREDS_USR}/msmicroservice:latest'
                    sh 'docker logout'
                }
            }
        }

        stage('Run Container') {
            steps {
                script {
                    sh 'docker login -u ${DOCKER_CREDS_USR} -p ${DOCKER_CREDS_PSW}'
                    sh 'docker rm msmicroservice -f'
                    sh 'docker run -d -p 8080:8080 --name msmicroservice ${DOCKER_CREDS_USR}/msmicroservice:$BUILD_NUMBER'
                    sh 'docker logout'
                }
            }
        }
    }
}
