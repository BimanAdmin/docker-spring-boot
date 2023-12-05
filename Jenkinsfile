pipeline {
    agent any

    environment {
        registry = "193088938549.dkr.ecr.us-east-1.amazonaws.com/qa-automation"
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/BimanAdmin/docker-spring-boot.git']])
            }
        }
        
        stage ("Build JAR") {
            steps {
                sh "sudo apt-get install maven -y"
                //sh "mvn clean install"
            }
        }
        
        stage ("Build Image") {
            steps {
                script {
                    sh 'sudo usermod -aG docker jenkins'
                    //sh 'sudo service jenkins restart'
                    dockerImage = docker.build registry
                    dockerImage.tag("$BUILD_NUMBER")
                }
            }
        }
        
        stage ("Push to ECR") {
            steps {
                script {
                    sh "aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 193088938549.dkr.ecr.us-east-1.amazonaws.com"
                    sh "docker push 193088938549.dkr.ecr.us-east-1.amazonaws.com/qa-automation:$BUILD_NUMBER"
                    
                }
            }
        }
        
    }
}
