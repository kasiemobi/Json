pipeline {
  agent any
  
  stages {
    stage('Checkout') {
      steps {
        checkout scm
      }
    }
    
    stage('Build') {
      steps {
        sh 'npm install' // install dependencies
        sh 'npm run build' // build the app
      }
    }
    
    stage('Test') {
      steps {
        sh 'npm run test' // run tests
      }
    }
    
    stage('Deploy') {
      steps {
        withCredentials([[
          $class: 'UsernamePasswordMultiBinding',
          credentialsId: 'my-credentials-id',
          usernameVariable: 'USERNAME',
          passwordVariable: 'PASSWORD'
        ]]) {
          sh 'npm run deploy -- --username $USERNAME --password $PASSWORD' // deploy app
        }
      }
    }
  }
  
  post {
    always {
      junit 'reports/**/*.xml' // publish test results
      archiveArtifacts 'dist/**/*' // archive built artifacts
    }
  }
}
