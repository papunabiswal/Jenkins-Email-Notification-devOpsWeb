pipeline {
    agent {
        label 'linux'
    }
    // tools {
    //     maven 'maven3'
    //     jdk 'jdk11'
    // }
    environment {
        REMOTE_USER = 'root' // Replace with your remote server user
        REMOTE_HOST = '172.31.7.2' // Replace with your Tomcat server's IP or hostname
        TOMCAT_HOME = '/opt/tomcat/' // Replace with the Tomcat installation path
        REMOTE_DIR = "${TOMCAT_HOME}/webapps/"
        BACKUP_DIR = "${TOMCAT_HOME}/backup/"
        FILE_NAME = 'devOpsWeb.war' // Replace with your application's WAR file name
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
                    withCredentials([sshUserPrivateKey(credentialsId: 'tomcat-cred', keyFileVariable: 'SSH_KEY')]) {
                        sh """
                        # Backup the existing WAR file if it exists
                        ssh -i ${SSH_KEY} -o StrictHostKeyChecking=no ${REMOTE_USER}@${REMOTE_HOST} \\
                            "if [ -f ${REMOTE_DIR}${FILE_NAME} ]; then \\
                                mv ${REMOTE_DIR}${FILE_NAME} ${BACKUP_DIR}${FILE_NAME}.\$(date +%F-%T); \\
                            fi"
                        # Copy the new WAR file to the Tomcat webapps directory
                        scp -i ${SSH_KEY} -o StrictHostKeyChecking=no ${WORKSPACE}/${FILE_NAME} ${REMOTE_USER}@${REMOTE_HOST}:${REMOTE_DIR}
                        """
                    }
                }
            }
        }

        stage('Restart Tomcat') {
            steps {
                script {
                    withCredentials([sshUserPrivateKey(credentialsId: 'your-ssh-key-id', keyFileVariable: 'SSH_KEY')]) {
                        sh """
                        ssh -i ${SSH_KEY} -o StrictHostKeyChecking=no ${REMOTE_USER}@${REMOTE_HOST} \\
                            "sudo systemctl restart ${TOMCAT_SERVICE}"
                        """
                    }
                }
            }
        }
    }
    
}
