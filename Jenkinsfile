pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        script {
          checkout scm


          def customImage = docker.build("${registry}:${env.BUILD_ID}")
        }

      }
    }

    stage('unit-test') {
      steps {
        script {
          docker.image("${registry}:${env.BUILD_UP}").inside{c -> sh 'python app_test'}
        }

      }
    }

  }
  environment {
    registry = 'secop/my-app'
  }
}