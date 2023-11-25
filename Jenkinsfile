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
    stage('Publish') {
      steps {
        script {
          docker.withRegistry('', 'dockerhub_id') {
            docker.image("${registry}:${env.BUILD_ID}").push("${env.BUILD_ID}")
          }
        }
      }
    }
    stage('Deploy') {
      steps {
        sh "docker stop flask-app || true; docker rm flask-app || true; docker run -d --name flask-app -p 9000:9000 ${registry}:${env.BUILD_ID}"
      }
    }
    stage('validate') {
      steps {
        sh 'sleep 5; curl -i http://192.168.1.15:9005/test_string'
      }
    }
  }
  environment {
    registry = 'secop/my-app'
  }
}
