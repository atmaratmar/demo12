pipeline {
    agent {
        docker {
            image 'maven:3.8.6-eclipse-temurin-17'
            args '-v $HOME/.m2:/root/.m2' // Cache Maven dependencies
        }
    }
    stages {
        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }
    }
}