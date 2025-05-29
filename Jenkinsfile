pipeline {
    agent any
    tools {
        jdk 'jdk17'
        maven 'maven-3.8.6'
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scm  // Explicit checkout
            }
        }
        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage('Docker Build') {
            steps {
                sh 'docker build -t your-app:${BUILD_NUMBER} .'
            }
        }
    }
}