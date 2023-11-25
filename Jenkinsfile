pipeline {
  agent any
  stages {
    stage('Check code quality') {
      steps {
        script {
          docker.image('python:3.9').inside {
            c -> sh '''
            python -m venv .venv
            . .venv/bin/activate
            pip install pylint
            pip install -r requirements.txt
            pylint --exit-zero --report=y --output-format=json:pylint-report.json,colorized ./*py
            '''
            publishHTML target : [
              allowMissing: true,
              alwaysLinkToLastBuild: true,
              keepAll: true,
              reportDir: './',
              reportFiles: 'pylint-report.json',
              reportName: 'pylint Scan',
              reportTitles: 'pylint AScan'
            ]
          }
        }
      }
    }
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
            docker.image("${registry}:latest").push('latest')
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
