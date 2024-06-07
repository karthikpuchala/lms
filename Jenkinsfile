/* groovylint-disable CompileStatic, NoDef */
pipeline {
    agent any
    environment {
        DOCKERHUB_CREDENTIALS = credentials('DOCKERHUB_CREDENTIALS')
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
                    // login into dockerhub
                    sh 'echo \$DOCKERHUB_CREDENTIALS_PSW | docker login -u \$DOCKERHUB_CREDENTIALS_USR--password-stdin'
                }
            }
        }
    }
}
// logout page 112
