pipeline {
  agent any

  environment {
    IMAGE_NAME = "rakhi12345/test"
  }

  stages {

    stage('Clone repository') {
      steps {
        checkout scm
      }
    }

    stage('Build image') {
      steps {
        script {
          app = docker.build("${IMAGE_NAME}")
        }
      }
    }

    stage('Test image') {
      steps {
        script {
          app.inside {
            sh 'echo "Tests passed"'
          }
        }
      }
    }

    stage('Push image') {
      steps {
        script {
          docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
            app.push("${env.BUILD_NUMBER}")
          }
        }
      }
    }

    stage('Trigger ManifestUpdate') {
      steps {
        echo "triggering updatemanifest job"
        build job: 'updatemanifest',
          parameters: [string(name: 'DOCKERTAG', value: env.BUILD_NUMBER)]
      }
    }
  }
}
 
