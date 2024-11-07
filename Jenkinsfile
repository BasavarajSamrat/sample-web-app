pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                // Check out code from GitHub
                git url: 'https://github.com/BasavarajSamrat/sample-web-app.git', branch: 'main'
            }
        }

        stage('Build Docker Image') {
            steps {
                // Build Docker image
                sh 'docker build -t sample-web-app:latest .'
            }
        }

        stage('Tag Image') {
            steps {
                // Tag Docker image for Docker Hub
                sh 'docker tag sample-web-app:latest basavarajsamrat/sample-web-app:latest'
            }
        }

        stage('Push Image to Docker Hub') {
            steps {
                // Use Docker Hub credentials to log in and push the image
                withCredentials([usernamePassword(credentialsId: 'docker-hub-creds', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                    sh 'docker push basavarajsamrat/sample-web-app:latest'
                }
            }
        }

        stage('Pull Image from Docker Hub') {
            steps {
                // Pull the image from Docker Hub
                sh 'docker pull basavarajsamrat/sample-web-app:latest'
            }
        }

        stage('Run Docker Container') {
            steps {
                // Remove any existing container and run a new one
                sh '''
                docker rm -f sample-web-app || true
                docker run -d -p 80:80 --name sample-web-app basavarajsamrat/sample-web-app:latest
                '''
            }
        }
    }

    post {
        always {
            echo 'Pipeline completed.'
        }
    }
}
