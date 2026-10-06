pipeline {
    agent any

     tools {
        maven 'maven3916'
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                bat 'mvn clean package'
            }
        }

        stage('Test') {
            steps {
                bat 'mvn test'
            }
        }

        stage('Archive') {
            steps {
                archiveArtifacts artifacts: 'target/payment-2.7.jar',
                                   fingerprint: true
            }
        }

        stage('Approval') {
            when {
                branch 'main'
            }
            steps {
                input message: 'Approve production deployment?',
                      ok: 'Deploy'
            }
        }

        stage('Deploy') {
            when {
                branch 'main'
            }
            steps {
                echo 'Deploying target/payment-2.7.jar'
            }
        }
    }

    post {

        always {
            junit 'target/surefire-reports/*.xml'
            deleteDir()
        }

        success {
            echo 'Deployment successful'
        }

        failure {
            echo 'Pipeline failed'
        }

        aborted {
            echo 'Deployment aborted by user'
        }
    }
}