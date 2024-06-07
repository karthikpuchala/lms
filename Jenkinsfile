/* groovylint-disable CompileStatic, NoDef */
pipeline {
    agent any
    environment {
        DOCKERHUB_CREDENTIALS = credentials('jenkin')
    }
    stages {
        stage('get package version') {
            steps {
                script {
                    /* groovylint-disable-next-line NoDef, VariableTypeRequired */
                    def packageJson = readJSON file: 'api/package.json'
                    def version = packageJson.version
                    env.PACKAGE_VERSION = version
                }
            }
        }
        stage('build image') {
            steps {
                script {
                    sh "cd api && sudo docker build -t karthikpuchala/lms:${env.PACKAGE_VERSION} ."
                }
            }
        }
        stage('login into dockerhub') {
            steps {
                script {
                    sh "echo \$DOCKERHUB_CREDENTIALS_PSW | docker login -u \$DOCKERHUB_CREDENTIALS_USR--Password-stdin"
                }
            }
        }
        stage('push the image') {
            steps {
                script {
                    sh "docker push karthikpuchala/lms:${env.PACKAGE_VERSION}"
                }
            }
        }
    }
}
// logout page 112
