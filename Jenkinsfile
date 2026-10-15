
pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install') {
            steps {
                echo 'Installing dependencies...'
            }
        }

        stage('Build') {
            steps {
                echo 'Building website...'
                sh 'test -f index.html'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploy stage ready'
            }
        }
    }

    post {
        success {
            echo 'BUILD SUCCESS'
        }
        failure {
            echo 'BUILD FAILED'
        }
    }
}
