/* groovylint-disable CompileStatic */
pipeline {
    agent any
    environment {
        DOCKERHUB_CREDENTIALS = credentials('lms')
    }
    stages {
        stage('get package version') {
            steps {
                script {
                    def packageJson = readJSON file: 'api/package.json'
                    def version = packageJson.version
                    env.PACKAGE_VERSION = version
                }
            }
        }
    }

        stage(build image) {
            steps {
                sh "cd api && docker build -t karthikpuchala/lms:${env.PACKAGE_VERSION} ."
            }
        }

        stage('login to dockerhub') {
            steps {
                sh "echo \$DOCKERHUB_CREDENTIALS_PSW | docker login -u \$DOCKERHUB_CREDENTIALS_USR --password-stdin"
            }
        }

        stage('push the image to dockerhub') {
            steps {
                sh "docker push karthikpuchala/lms:${env.PACKAGE_VERSION}"
            }
        post {
            always {
                sh 'docker logout'
            }
        }
        }
}
// logout
