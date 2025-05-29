pipeline {
    agent any

    tools {
        jdk 'jdk17'       // Must match Jenkins Global Tool config
        maven 'maven-3.8.6'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
                sh 'ls -la'  // Verify files exist
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Docker') {
            steps {
                sh '''
                docker build -t demo12:${BUILD_NUMBER} .
                docker images | grep demo12
                '''
            }
        }
    }
}