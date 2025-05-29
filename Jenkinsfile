pipeline {
    agent any
    tools {
        jdk 'jdk17'
        maven 'maven-3.8.6'
    }
    stages {
        stage('Checkout & Verify') {
            steps {
                checkout scm
                sh 'ls -la && mvn --version'
            }
        }
        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage('Docker Build') {
            steps {
                sh 'docker build -t demo12:${BUILD_NUMBER} .'
                sh 'docker images | grep demo12'
            }
        }
    }
}
