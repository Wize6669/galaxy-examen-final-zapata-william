pipeline {
    agent any
    environment {
        DOCKER_CREDS = credentials('docker-credentials')
    }

    stages {
        stage('Test') {
            steps {
                sh 'docker ps -a'
            }
        }

        stage('Build') {
            agent {
                docker { image 'maven:3.6.3-openjdk-11-slim' }
            }
            steps {
                sh 'ls -l target/'
                sh 'mvn clean install'
                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
            }
        }

        stage('SonarQube') {
            steps {
                script {
                    def scannerHome = tool 'scanner-default'
                    withSonarQubeEnv('sonar-server') {
                        sh "${scannerHome}/bin/sonar-scanner " +
                           "-Dsonar.projectKey=msmicroservice " +
                           "-Dsonar.projectName=msmicroservice " +
                           "-Dsonar.sources=src/main/kotlin " +
                           "-Dsonar.java.binaries=build/classes " +
                           "-Dsonar.tests=src/test/kotlin"
                    }
                }
            }
        }

        stage('Build Image') {
            steps {
                copyArtifacts filter: 'target/*.jar',
                            fingerprintArtifacts: true,
                            projectName: '${JOB_NAME}',
                            flatten: true,
                            selector: specific('${BUILD_NUMBER}'),
                            target: 'target/'
                sh 'which docker' 
                sh 'docker --version'
                sh 'docker-compose --version'
                sh 'docker-compose build'
            }
        }

        stage('Publish Image') {
            steps {
                script {
                    sh 'docker login -u ${DOCKER_CREDS_USR} -p ${DOCKER_CREDS_PSW}'
                    sh 'docker tag msmicroservice ${DOCKER_CREDS_USR}/msmicroservice:$BUILD_NUMBER'
                    sh 'docker push ${DOCKER_CREDS_USR}/msmicroservice:$BUILD_NUMBER'
                    sh 'docker logout'
                }
            }
        }

        stage('Run Container') {
            steps {
                script {
                    sh 'docker login -u ${DOCKER_CREDS_USR} -p ${DOCKER_CREDS_PSW}'
                    sh 'docker rm msmicroservice -f || true'
                    sh 'docker run -d -p 8080:8080 --name msmicroservice ${DOCKER_CREDS_USR}/msmicroservice:$BUILD_NUMBER'
                    sh 'docker logout'
                }
            }
        }
    }
}
