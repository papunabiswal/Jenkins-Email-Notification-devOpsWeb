pipeline {

  agent any
  
  stages{
    stage ('Build'){
      steps{
        echo "Builing the project"
        // sh 'ajbfagfiaf'
    }
  }
  }
    post{
      success{
        emailext body: 'Email send out from Jenkins', subject: 'Test email', to: 'papunabiswal55@gmail.com'
        // emailext attachLog: true, body: 'Email sent out from Jenkins', subject: '$PROJECT_NAME - Build # $BUILD_NUMBER - $BUILD_STATUS!', to: 'papunabiswal55@gmail.com'
      }
      // failure{
      //   emailext attachLog: true, body: 'Email sent out from Jenkins', subject: '$PROJECT_NAME - Build # $BUILD_NUMBER - $BUILD_STATUS!', to: 'papunabiswal55@gmail.com'
      // }
    }
}
