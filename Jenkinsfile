pipeline {

    environment {
        AWS_ACCESS_KEY_ID = credentials('AWS_ACCESS_KEY')
        AWS_SECRET_ACCESS_KEY = credentials('AWS_SECRET_ACCESS_KEY')
    }
    agent any
    stages {

        stage('checkout') {
            steps {
                 script{
                        dir("eks-jenkins")
                        {
                            git branch: 'main', url: 'https://github.com/Tanmoy009/eks-jenkins.git'
                        }
                    }
                }
            }

        stage ('test') {
            steps {       
                sh 'go build -o go-web-app'
                sh 'go test ./...'        
                sh 'golint ./...'
            }
        }
        
        stage ('build') {
            steps {
                sh 'docker build -t public.ecr.aws/p3b6w3x9/eks-cluster:v4 .'
                sh 'aws ecr-public get-login-password --region us-east-1 | docker login --username AWS --password-stdin public.ecr.aws/p3b6w3x9'
                sh 'docker push public.ecr.aws/p3b6w3x9/eks-cluster:v4'
            }
        }

    }
    post {
        success {
            echo 'Code quality tests passed!'
        }
        failure {
            echo 'Code quality tests failed!'
        }
    }

}