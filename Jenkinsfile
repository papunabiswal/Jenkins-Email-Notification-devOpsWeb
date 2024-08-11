pipeline {
    agent {
        label 'linux'
    }
    // tools {
    //     maven 'maven3'
    //     jdk 'jdk11'
    // }
    environment {
        REMOTE_USER = 'root'
        REMOTE_HOST = '172.31.7.2'
        REMOTE_DIR = '/opt/tomcat/webapps/'
        BACKUP_DIR = '/tmp/backup/'
        WORKSPACE = '/home/root/workspace/pipeline/target/'
        FILE_NAME = 'devOpsWeb.war'
        TOMCAT_SERVICE = 'tomcat'
    }

    stages {
        stage('Git checkout') {
            steps {
                git branch: 'DevOps', credentialsId: 'GitHub-Cred', url: 'https://github.com/papunabiswal/Jenkins-Email-Notification-devOpsWeb.git'
            }
        }
        
        stage('Build') {
            steps {
                sh "mvn clean package"
            }
        }

        stage('Deploy to Remote Server') {
            steps {
                script {
                    // Copy the file to the remote server, backing up the existing file and deploying the new one
                    sh """
                    # Backup the existing file if it exists
                    ssh -o StrictHostKeyChecking=no ${REMOTE_USER}@${REMOTE_HOST} \\
                        "if [ -f ${REMOTE_DIR}${FILE_NAME} ]; then \\
                            mv ${REMOTE_DIR}${FILE_NAME} ${BACKUP_DIR}${FILE_NAME}.\$(date +%F-%T); \\
                        fi"

                    # Copy the new file to the remote server
                    scp -o StrictHostKeyChecking=no ${WORKSPACE}/${FILE_NAME} ${REMOTE_USER}@${REMOTE_HOST}:${REMOTE_DIR}
                    """
                }
            }
        }

        stage('Restart Tomcat') {
            steps {
                script {
                    // Restart Tomcat on the remote server
                    sh """
                    ssh -o StrictHostKeyChecking=no ${REMOTE_USER}@${REMOTE_HOST} \\
                        "sudo systemctl restart ${TOMCAT_SERVICE}"
                    """
                }
            }
        }
    }
    
}
