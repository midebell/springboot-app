pipeline {
    agent {
        docker {
            image 'midebell/dev-tools:v2'
            args '-v /var/run/docker.sock:/var/run/docker.sock'
        }
    }

    environment {
        registry = "411317891806.dkr.ecr.us-east-1.amazonaws.com/chijiokerepo"
        registryCredential = 'was'
        dockerImage = ''
    }
    
    stages {
        stage('Checkout') {
            steps {
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/Oka4chijioke/springboot-app.git']])
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
        
        // stage ("Deploy to K8S") {
        //     steps {
        //         withKubeConfig(caCertificate: '', clusterName: '', contextName: '', credentialsId: 'K8S', namespace: '', restrictKubeConfigAccess: false, serverUrl: '') {
        //         sh "kubectl apply -f eks-deploy-k8s.yaml"
                    
        //         }
        //     }
        // }
    }
}
