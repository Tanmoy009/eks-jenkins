pipeline {

    environment {
        AWS_ACCESS_KEY_ID = 
        AWS_SECRET_ACCESS_KEY = 
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
                sh 'golint ./...'
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