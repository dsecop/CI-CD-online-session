pipeline {
  agent any
  stages {
    stage('Build') {
      agent any
      steps {
        script {
          checkout scm
          def customImage = docker.build("${registry}:${env.BUILD_ID}")
        }

      }
    }

  }
  environment {
    registry = 'secop/my-app'
  }
}