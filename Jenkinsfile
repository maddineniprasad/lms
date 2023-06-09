pipeline {
    agent any
    environment {
        // More detail: 
        // https://jenkins.io/doc/book/pipeline/jenkinsfile/#usernames-and-passwords
        NEXUS_CRED = credentials('nexus')
   }

    stages {
        stage('sonar analysis') {
            steps {
                echo 'testing..'
                sh 'whoami'
                sh 'sudo apt update -y'
            }
        }

        stage('Build') {
            steps {
                echo 'Building..'
                sh 'cd webapp && npm install && npm run build'
                sh 'npm install'
            }
        }
        stage('Test') {
        stage('release') {
            steps {
                echo 'Testing..'
                sh 'cd webapp && sudo docker container run --rm -e SONAR_HOST_URL="http://35.154.46.45:9000" -e SONAR_LOGIN="sqp_6453ee741397c60d3b0a7568aa58fef855ff8152" -v ".:/usr/src" sonarsource/sonar-scanner-cli -Dsonar.projectKey=lms'
                echo 'building..'
            }
        }
        stage('Release') {
        stage('Deploy') {
            steps {
                echo 'Release Nexus'
                sh 'rm -rf *.zip'
                sh 'cd webapp && zip dist-${BUILD_NUMBER}.zip -r dist'
                sh 'cd webapp && curl -v -u $Username:$Password --upload-file dist-${BUILD_NUMBER}.zip http://35.154.46.45:8081/repository/lms/'
                echo 'Deploying....'
            }
        }
    }