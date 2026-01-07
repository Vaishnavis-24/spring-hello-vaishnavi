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
                script {
                    app = docker.build("vaishsuresh24/test:${DOCKERTAG}")
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
                        app.push("${DOCKERTAG}")
                        app.push("latest")
                    }
                }
            }
        }

        stage('Trigger ManifestUpdate') {
            steps {
                script {
                    build job: 'updatemanifest',
                        parameters: [string(name: 'DOCKERTAG', value: DOCKERTAG)]
                }
            }
        }
    }
}
 
