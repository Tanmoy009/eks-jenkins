pipeline {

    environment {
        AWS_ACCESS_KEY_ID = 
        AWS_SECRET_ACCESS_KEY = 
    }
    agent any
    stages {
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