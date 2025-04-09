pipeline {
    agent {
        label "Jenkins-Agent"
    }
    tools {
        maven 'Maven3'
    }
    stages {

        stage('Clean Workspace') {
            steps {
                cleanWs()
            }
        }

        stage('Git Checkout') {
            steps {
                git branch: 'main', credentialsId: 'git-cred', url: 'https://github.com/marviflame/my-java-app-project.git'
            }
        }

        stage('Compile') {
            steps {
                sh 'mvn clean compile'
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }

        stage('Trivy Scan') {
            steps {
                sh 'trivy fs --format table -o fs-report.html .'
            }
        }
    }
}
