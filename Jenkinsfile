pipeline {
    agent {
        docker {
            image 'midebell/dev-tools:v2'
            args '-v /var/run/docker.sock:/var/run/docker.sock'
        }
    }

    environment {
        registry = "411317891806.dkr.ecr.us-east-1.amazonaws.com/chijiokerepo"
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
                    docker.build registry
                }
            }
        }
        
        stage ("Push to ECR") {
            steps {
                withAWS(credentials: 'aws', region: 'us-east-1') {
                    sh "aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 411317891806.dkr.ecr.us-east-1.amazonaws.com"
                    sh "docker push 411317891806.dkr.ecr.us-east-1.amazonaws.com/chijiokerepo:latest"
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
