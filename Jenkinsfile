pipeline {
    agent any

    stages {
        stage('build docker imager') {
            steps {
                script{
                    sh 'docker build -t karthikpuchala/dck .'
                }
            }
        }
    }
}



