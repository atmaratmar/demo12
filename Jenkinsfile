pipeline {
    agent any

    // 1. TOOL CONFIGURATION (or use Docker agents below)
    tools {
        jdk 'jdk17'          // Must match Jenkins Global Tool Configuration
        maven 'maven-3.8.6'   // Must match exactly
    }

    environment {
        // 2. DOCKER CONFIG
        DOCKER_IMAGE = "your-dockerhub-username/demo12"
        DOCKER_TAG = "${env.BUILD_NUMBER}"
        DOCKER_REGISTRY_CREDENTIALS = 'dockerhub-creds' // Jenkins credentials ID
    }

    stages {
        // 3. CHECKOUT STAGE
        stage('Checkout SCM') {
            steps {
                checkout scm
                sh 'ls -la' // Verify files
            }
        }

        // 4. BUILD STAGE
        stage('Build with Maven') {
            steps {
                sh 'mvn clean package -DskipTests'
                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
            }
        }

        // 5. DOCKER BUILD
        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${DOCKER_IMAGE}:${DOCKER_TAG}")
                }
            }
        }

        // 6. DEPLOY TO SWARM
        stage('Deploy to Swarm') {
            when {
                branch 'main' // Only deploy from main branch
            }
            steps {
                script {
                    // 7. LOGIN TO DOCKER REGISTRY (if using private repo)
                    withCredentials([usernamePassword(
                        credentialsId: DOCKER_REGISTRY_CREDENTIALS,
                        usernameVariable: 'DOCKER_USER',
                        passwordVariable: 'DOCKER_PASS'
                    )]) {
                        sh "echo ${DOCKER_PASS} | docker login -u ${DOCKER_USER} --password-stdin"
                    }

                    // 8. DEPLOY COMMANDS
                    sh '''
                        docker stack rm demo12 || true
                        docker stack deploy \
                            --with-registry-auth \
                            -c docker-compose.yml \
                            demo12
                    '''
                }
            }
        }
    }

    // 9. POST-ACTIONS (Cleanup & Notifications)
    post {
        always {
            cleanWs() // Clean workspace
        }
        success {
            slackSend channel: '#deployments',
                     message: "SUCCESS: ${env.JOB_NAME} ${env.BUILD_NUMBER}"
        }
        failure {
            slackSend channel: '#errors',
                     message: "FAILED: ${env.JOB_NAME} ${env.BUILD_NUMBER}"
        }
    }
}