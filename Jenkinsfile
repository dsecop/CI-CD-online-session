pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        script {
          checkout scm


          def customImage = docker.buildx("${registry}:${env.BUILD_ID}")
        }

      }
    }

  }
  environment {
    registry = 'secop/my-app'
  }
}