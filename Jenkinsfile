pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                // Checkout the source code from the repository
                checkout scm
            }
        }

        stage('Build') {
            steps {
                script {
                    // Pull Jenkins Docker image and start container if not already running
                    sh 'docker pull jenkins/jenkins:lts'
                    sh 'docker run -d -p 8080:8080 -p 50000:50000 --name my-jenkins jenkins/jenkins:lts'
                    // Other build steps as needed
                }
            }
        }

        stage('Test Application') {
            steps {
                script {
                    // Build application test image using Dockerfile.test
                    sh 'docker build -f Dockerfile.test -t my-app-test .'
                    // Run tests in the Docker container using test_main.py
                    sh 'docker run --rm my-app-test python test_main.py'
                }
            }
        }

        // Other stages for deployment, etc.
    }

    post {
        always {
            // Clean up or post-build tasks
            script {
                // Stop and remove Jenkins container
                sh 'docker stop my-jenkins'
                sh 'docker rm my-jenkins'
            }
        }
    }
}
