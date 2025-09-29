pipeline {
    agent any

    environment {
        // Docker Hub credentials stored in Jenkins Credentials (type: username/password)
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-credentials-id')
        IMAGE_NAME = 'your-dockerhub-username/my-maven-app'
        IMAGE_TAG = 'latest'
    }

    tools {
        // Use any Java installed on Jenkins node (must be >= 17)
        jdk 'Java17' 
        maven 'Maven3'
    }

    stages {
        stage('Pull Code') {
            steps {
                git branch: 'main', url: 'https://github.com/om0508/my-maven-app.git'
            }
        }

        stage('Build with Maven') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "docker build -t ${IMAGE_NAME}:${IMAGE_TAG} ."
            }
        }

        stage('Push Docker Image') {
            steps {
                sh "echo ${DOCKERHUB_CREDENTIALS_PSW} | docker login -u ${DOCKERHUB_CREDENTIALS_USR} --password-stdin"
                sh "docker push ${IMAGE_NAME}:${IMAGE_TAG}"
            }
        }

        stage('Run Docker Container') {
            steps {
                // Stop/remove container if already running
                sh "docker rm -f my-maven-app || true"
                sh "docker run -d -p 8080:8080 --name my-maven-app ${IMAGE_NAME}:${IMAGE_TAG}"
            }
        }
    }

    post {
        success {
            echo "CI/CD pipeline completed successfully!"
        }
        failure {
            echo "Pipeline failed. Check logs for details."
        }
    }
}
