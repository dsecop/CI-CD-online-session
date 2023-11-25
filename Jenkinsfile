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

    stage('unit-test') {
      agent any
      environment {
        registry = 'secop/my-app'
      }
      steps {
        script {
          docker.image("${registry}:${env.BUILD_ID}").inside{c-> sh 'python3 app_test.py'}
        }

      }
    }

  }
  environment {
    registry = 'secop/my-app'
  }
}