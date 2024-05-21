pipeline {
    agent any

    stages {
        stage('LMS-Code-Analysis') {
            steps {
                echo 'Sonar Analysis'
                sh 'cd webapp && sudo docker run  --rm -e SONAR_HOST_URL="http://3.25.50.161:9000" -e SONAR_LOGIN="sqp_1ab0461d743ce1cf207c23c7770ed655190b47da"  -v ".:/usr/src" sonarsource/sonar-scanner-cli -Dsonar.projectKey=lms'
                echo 'Analysis Completed'
            }
        }

        stage('LMS-Build') {
            steps {
                echo 'Building LMS'
                sh 'cd webapp && npm install && npm run build'
                echo 'Building Completed'
            }
        }

        stage('LMS-Release') {
            steps {
                script {
                    echo "Publish LMS Artifacts"       
                    def packageJSON = readJSON file: 'webapp/package.json'
                    def packageJSONVersion = packageJSON.version
                    echo "${packageJSONVersion}"  
                    sh "zip webapp/lms-${packageJSONVersion}.zip -r webapp/dist"
                    sh "curl -v -u admin:lms1234 --upload-file webapp/lms-${packageJSONVersion}.zip http://3.25.50.161:9000 /repository/lms/"     
                }
            }
        }

        stage('LMS-Deploy') {
            steps {
                script {
                     echo "Deploy LMS"       
                    def packageJSON = readJSON file: 'webapp/package.json'
                    def packageJSONVersion = packageJSON.version
                    echo "${packageJSONVersion}"  
                    sh "curl -u admin:lms1234 -X GET \'http://3.25.50.161:8081/repository/lms/lms-${packageJSONVersion}.zip\' --output lms-'${packageJSONVersion}'.zip"
                    sh 'sudo rm -rf /var/www/html/*'
                    sh "sudo unzip -o lms-'${packageJSONVersion}'.zip"
                    sh "sudo cp -r webapp/dist/* /var/www/html"
            }
            }
        }
    }
}