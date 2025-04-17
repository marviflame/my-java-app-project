pipeline {
    agent {
        label "agent"
    }
    tools {
        maven 'Maven3'
    }

    environment{
        APP_NAME = 'java-app-project'
        APP_VERSION = '1.0.0'
        DOCKER_USER = 'marviflame89'
        DOCKER_PASS = 'dockerhub'
        IMAGE_TAG = "${APP_VERSION}-${BUILD_NUMBER}"
        DOCKER_IMAGE = "${DOCKER_USER}/${APP_NAME}:${IMAGE_TAG}"
        
    }

    stages {

        stage('Clean Workspace') {
            steps {
                cleanWs()
            }
        }

        stage('Git Checkout') {
            steps {
                git branch: 'main', credentialsId: 'git-login', url: 'https://github.com/marviflame/my-java-app-project.git'
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

        stage('Sonarqube Analysis') {
            steps {
                script {
                    withSonarQubeEnv(credentialsId: 'sonar-token') {
                       sh 'mvn sonar:sonar'
                }
                
                }
            }
        }
        
        stage('Sonarqube Quality Gate') {
            steps {
                script {
                    waitForQualityGate abortPipeline: false, credentialsId: 'sonar-token'
                }
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Docker Build & Push') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'dockerhub') {
                        sh "docker build -t ${DOCKER_IMAGE} ."
                        sh "docker push ${DOCKER_IMAGE}"
                    }
                }
            }
        }
    }    
}
