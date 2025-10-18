pipeline {
    agent any

    tools {
        jdk 'JDK17'
        maven 'Maven3'
    }

    stages {
        stage('Checkout') {
            // write your logic here
        }
        stage('Build') {
            // write your logic here
        }
        stage('Run Application') {
            // write your logic here
        }
        stage('Test') {
            // write your logic here
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
    }
}