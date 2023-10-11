pipeline {
    agent any
    tools {
        maven 'maven3'
        jdk 'jdk17'
    }

    stages {
        stage('Git checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Saish40/Ekart.git'
            }
        }

        stage('Compile') {
            steps {
                sh 'mvn compile'
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test -DskipTests=true'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package -DskipTests=true'
            }
        }
    }
}