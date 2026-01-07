def app

pipeline {
    agent any

    stages {

        stage('Clone repository') {
            steps {
                checkout scm
            }
        }

        stage('Build image') {
            steps {
                sh "docker build -t vaishsuresh24/test:${BUILD_NUMBER} ."
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
                sh "echo $DOCKER_PASSWORD | docker login -u $DOCKER_USERNAME --password-stdin"
                sh "docker push vaishsuresh24/test:${BUILD_NUMBER}"
                sh "docker tag vaishsuresh24/test:${BUILD_NUMBER} vaishsuresh24/test:latest"
                sh "docker push vaishsuresh24/test:latest"
            }
         }


        stage('Trigger ManifestUpdate') {
            steps {
                script {
                    build job: 'updatemanifest',
                        parameters: [string(name: 'DOCKERTAG', value: BUILD_NUMBER)]
                }
            }
        }
    }
}
 
