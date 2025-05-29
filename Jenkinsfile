pipeline {
    agent any
    tools {
        jdk 'jdk17'
        maven 'maven-3.8.6'
    }
    stages {
        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }
    }
}