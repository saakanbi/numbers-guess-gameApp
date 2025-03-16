pipeline {
    agent any
    environment {
        SONARQUBE_URL = "http://13.53.123.95:9000"
    }
    parameters {
        string(name: 'DEPLOY_ENV', defaultValue: 'dev', description: 'Deployment Environment')
        string(name: 'BRANCH', defaultValue: 'dev', description: 'Git Branch to build')
    }
    stages {
        stage('Checkout Code') {
            steps {
                script {
                    git branch: "dev",
                        url: 'https://github.com/saakanbi/numbers-guess-gameApp.git'
                }
            }
        }
        stage('Build') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }
        stage('SonarQube Analysis') {
            steps {
                script {
                    withSonarQubeEnv('sonarqube-token') {
                        sh '''
                        mvn sonar:sonar \
                        -Dsonar.projectKey=NumbersGuessGame \
                        -Dsonar.projectName="NumbersGuessGame" \
                        -Dsonar.host.url=${SONARQUBE_URL} \
                        -Dsonar.login=${SONAR_AUTH_TOKEN}
                        '''
                    }
                }
            }
        }
        stage('Deploy to Tomcat') {
            steps {
                script {
                    deploy adapters: [tomcat7(
                        credentialsId: 'TOMCATID',
                        path: '',
                        url: 'http://3.142.36.180:8080'
                    )],
                    contextPath: 'numbers-game', war: 'target/*.war'
                }
            }
        }
    }
    post {
        success {
            echo ':white_check_mark: Build, Testing, SonarQube Analysis, and Deployment Successful!'
        }
        failure {
            echo ':x: Build Failed! Check logs for issues.'
        }
    }
}

React

Reply












