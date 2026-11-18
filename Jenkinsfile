pipeline {
    agent any

    tools {
        jdk 'jdk17'
        maven 'maven3'
    }

    triggers {
        pollSCM('H/2 * * * *')
    }

    stages {
        stage('Checkout') {
            steps {
                sh 'git rev-parse HEAD'
                sh 'git branch --show-current'
            }
        }
        stage('Compile') {
            steps {
                sh 'mvn -B clean compile'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn -B test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
        stage('Package') {
            steps {
                sh 'mvn -B package -DskipTests'
            }
        }
        stage('Archive') {
            steps {
                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
            }
        }
    }

    post {
        success { echo 'BUILD SUCCESS - artifact archived' }
        failure { echo 'BUILD FAILED - see failing stage above' }
    }
}