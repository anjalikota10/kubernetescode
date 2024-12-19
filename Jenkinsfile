pipeline {
    agent any

    tools {
        maven 'maven'
        jdk 'jdk17'
    }

    environment {
        SCANNER_HOME = tool 'sonar-scanner'
    }

    stages {
        stage('Git Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/anjalikota10/Blogging-app.git'
            }
        }


        stage('Trivy FS Scan') {
            steps {
                sh 'trivy fs --format table -o fs-report.html .'
            }
        }
        
        stage('Build & Tag Docker Image') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'dockerhub', toolName: 'docker') {
                        sh 'docker build -t devops830/blogging-app:latest .'
                    }
                }
            }
        }

        stage('Scan Docker Image by Trivy') {
            steps {
                sh 'trivy image --format table -o image-report.html devops830/blogging-app:latest'
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'dockerhub', toolName: 'docker') {
                        sh 'docker push devops830/blogging-app:latest'
                    }
                }
            }
        }
    }
