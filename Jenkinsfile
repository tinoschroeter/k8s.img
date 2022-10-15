pipeline {
  agent any
  options {
      timeout(time: 14, unit: 'MINUTES')
  }
  stages {
      stage('Linting') {
          steps {
          echo 'linting..'
          sh 'printenv'
          }
      }
      stage('Build Production') {
        when { 
          branch 'master'
          anyOf {
            changeset "**"
          }
        }
        steps {
            echo 'Build Production....'
            sh("cd k3s/production/ && skaffold run")
          }  
        }
      stage('Build Docs') {
        when { changeset "README.md" }
        steps {
            echo 'Build Docs...'
          }  
        }
      }
      post {
        success {
           echo "Build successfully..."
           slackSend color: "good", message: "Build successfully on $JOB_NAME..."
       }
       failure {
           echo "Build failed..."
           slackSend color: "danger", message: "Build failed on $JOB_NAME..."
       }
    }
}
