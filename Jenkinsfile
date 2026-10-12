pipeline {
    agent any

    environment {
        APP_NAME = 'myapp'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
                sh 'ls -la'
            }
        }
        stage('Build') {
            steps {
                sh 'mkdir -p build && echo "${APP_NAME} v1.${BUILD_NUMBER}" > build/app.txt'
            }
        }
        stage('Test') {
            steps {
                sh 'test -f build/app.txt && echo "Test passed"'
            }
        }
        stage('Package') {
            steps {
                sh 'tar czf ${APP_NAME}-${BUILD_NUMBER}.tar.gz build/'
                archiveArtifacts artifacts: '*.tar.gz', fingerprint: true
            }
        }
        stage('Deploy') {
            steps {
                sh 'mkdir -p /var/jenkins_home/deploy && cp ${APP_NAME}-${BUILD_NUMBER}.tar.gz /var/jenkins_home/deploy/'
            }
        }
    }

    post {
        success { echo 'Pipeline succeeded' }
        failure { echo 'Pipeline failed' }
    }
}