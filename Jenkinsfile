pipeline {
    agent {
        label "Jenkins-Agent"
    }

    stages {

        stage('Clean Workspace') {
            steps {
                cleanWs()
            }
        }

        stage('Git Checkout') {
            steps {
                git branch: 'masin', credentialsId: 'git-cred', url: 'https://github.com/marviflame/my-java-app-project.git'
            }
        }
    }
}
