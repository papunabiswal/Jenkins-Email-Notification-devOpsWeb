pipeline {
    agent {
        label 'linux'
    }
    tools {
        maven 'maven3'
        jdk 'jdk11'
    }

    stages {
        stage('Git checkout') {
            steps {
                git branch: 'DevOps', url: 'https://github.com/papunabiswal/Jenkins-Email-Notification-devOpsWeb.git'
            }
        }
        
        stage('Build') {
            steps {
                sh "mvn clean package"
            }
        }
    }
    
}
