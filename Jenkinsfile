pipeline {

    agent {
        docker {
            image 'midebell/dev-tools:v2'
        }
    }

    environment {
        registry = "614292846436.dkr.ecr.us-east-1.amazonaws.com/midebell"
        registryCredential = 'was'
        dockerImage = ''
    }
    
    stages {
        stage('Checkout') {
            steps {
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: 'Jenkins-gitlab', url: 'https://git.lumiefy.com/jenkins/springboot-app.git']])
            }
        }
        
        stage ("build Jar") {
            steps {
                sh "mvn clean install"
            }
        }

        stage ("Build image") {
            steps {
                script {
                    dockerImage = docker.build registry
                }
            }
        }
        stage('Deploy image') {
            steps{
                script{
                    docker.withRegistry("https://" + registry, "ecr:us-east-1:" + registryCredential) {
                        dockerImage.push()
                    }
                }
            }
        }

        stage ("Deploy to K8S") {
            steps {
                sh "kubectl --version"    
            }
        }

        
    }
}
