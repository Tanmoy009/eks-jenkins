pipeline {
    agent any

    
    triggers {
        githubPullRequest()
    }

    options {
        buildDiscarder(logRotator(numToKeepStr: '5'))
    }

    stages {
        stage('Build') {
            agent {
                docker {
                    image 'golang:1.22'
                }
            }
            steps {
                sh 'go build -o go-web-app'
                sh 'go test ./...'
            }
        }
        stage('Code Quality') {
            agent {
                docker {
                    image 'golangci/golangci-lint:v1.56.2'
                }
            }
            steps {
                sh 'golangci-lint run'
            }
        }
        stage('Push to ECR') {
            agent {
                docker {
                    image 'aws/aws-cli'
                }
            }
            environment {
                AWS_ACCESS_KEY_ID = credentials('AWS_ACCESS_KEY_ID')
                AWS_SECRET_ACCESS_KEY = credentials('AWS_SECRET_ACCESS_KEY')
                AWS_REGION = 'ap-south-1'
            }
            steps {
                sh 'aws ecr-public get-login-password --region us-east-1 | docker login --username AWS --password-stdin public.ecr.aws/p3b6w3x9'
                sh 'docker build -t public.ecr.aws/p3b6w3x9/eks-cluster:${BUILD_NUMBER} .'
                sh 'docker push public.ecr.aws/p3b6w3x9/eks-cluster:${BUILD_NUMBER}'
            }
        }
        stage('Update Helm Chart') {
            agent any
            steps {
                sh 'sed -i "s/tag: .*/tag: \"${BUILD_NUMBER}\"/" helm/go-web-app-chart/values.yaml'
                sh 'git config --global user.email "tanmoycoc44@gmail.com"'
                sh 'git config --global user.name "Tanmoy009"'
                sh 'git add helm/go-web-app-chart/values.yaml'
                sh 'git commit -m "Update tag in Helm chart"'
                sh 'git push'
            }
        }
    }
}
