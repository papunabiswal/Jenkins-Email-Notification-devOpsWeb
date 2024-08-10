pipeline {
    agent {
        label 'linux'
    }
    
    tools {
        maven "maven3"
        jdk "jdk17"
    }
    // parameters {
    //      string(name: 'staging_server', defaultValue: '13.232.37.20', description: 'Remote Staging Server')
    // }

stages{
        stage('Git Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/papunabiswal/Ekart.git' 
            }
        }
        
        stage('Build'){
            steps {
                sh 'mvn clean package'
            }
            post {
                success {
                    echo 'Archiving the artifacts'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }

        stage ('Deployments'){
            parallel{
                stage ("Deploy to Staging"){
                    steps {
                        sh "scp -v -o StrictHostKeyChecking=no **/*.war root@172.31.5.58:/opt/tomcat/webapps/"
                    }
                }
            }
        }
    }
}
