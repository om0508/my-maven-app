pipeline {
    agent any
    tools {
        jdk 'OpenJDK 11'
    }
    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/om0508/my-maven-app.git'
            }
        }
        stage('Build') {
            steps {
                sh 'mvn clean install'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }
        stage('Deploy') {
            steps {
                sh 'mvn deploy'
            }
        }
    }
}
