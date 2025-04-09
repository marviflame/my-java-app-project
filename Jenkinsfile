pipeline {
    agent {
        label "Jenkins-Agent"
    }

    stages {
        stage('Git Checkout') {
            steps {
                git branch: 'main', credentialsId: 'git-cred', url: 'git@github.com:marviflame/my-java-app-project.git'
            }
        }
    }

    stages {
        stage('Clean Workspace') {
            steps {
                cleanWs()
            }
        }
    }
}
