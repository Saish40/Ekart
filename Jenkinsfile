pipeline {
    agent any
    tools {
        maven 'maven3'
        jdk 'jdk17'
    }
    environment {
        SCANNER_HOME = tool 'sonar-scanner'
    }
    stages {
        stage('Git checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Saish40/Ekart.git'
            }
        }

        stage('Compile') {
            steps {
                sh 'mvn clean compile -DskipTests=true'
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test -DskipTests=true'
            }
        }

        stage('Sonarqube') {
            steps {
                withSonarQubeEnv('sonar'){
                   sh ''' $SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=Shopping-Cart \
                   -Dsonar.java.binaries=. \
                   -Dsonar.projectKey=Shopping-Cart '''
               }
            }
        }

        stage('JAVA Build') {
            steps {
                sh 'mvn clean package -DskipTests=true'
            }
        }

        stage('Docker build') {
            steps {
                script{                        
                        sh "docker build -t shopping-cart -f docker/Dockerfile ."
                        sh "docker tag shopping-cart Saish69/shopping-cart:latest"
                       
            }
        }
    }

     stage('Docker login and push') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'dockerpwd', usernameVariable: 'dockerusr')]) {
                sh "docker login -u ${env.dockerusr} -p ${env.dockerpwd}"
                }
                sh "docker push saish69/shopping-cart:latest"
            }
        }

    stage('Docker deploy') {
            steps {
                script{
                        sh "docker run -d -p 8070:8070 Saish69/shopping-cart:latest"
            }
        }
    }
}
}
