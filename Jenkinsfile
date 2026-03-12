pipeline {
    agent {
          node {
            label 'AGENT-1'
          }
    }
    environment {
        Course = 'jenkins'
        appVersion = ""
        ACC_ID = "883238867597"
        PROJECT = "roboshop"
        COMPONENT = "catalogue"
    }
    options {
        timeout (time: 120, unit: 'SECONDS')
        disableConcurrentBuilds()
    }
    
    stages {
        stage('Read Version') {
            steps {
                script{
                    def packageJSON = readJSON file: 'package.json'
                    appVersion = packageJSON.version
                    echo "app version: ${appVersion}"
                 }
            }
        }
        stage('Install Dependenices') {
            steps {
                script {
                    sh """
                        npm install
                    """
            }
        }
        }
         stage('Unit Testing') {
            steps {
                script {
                    sh """
                        npm test
                    """
            }
        }
        }
        stage('Sonar Scan'){
            environment {
                def scannerHome = tool 'sonar8.0'
            }
            steps {
                script{
                    withSonarQubeEnv('Sonar') {
                        sh  "${scannerHome}/bin/sonar-scanner"
                    }
                }
            }
        }
        stage('Quality gate'){
            steps {
                timeout(time: 1, unit: 'HOURS') {
                    // Wait for the quality gate status
                    // abortPipeline: true will fail the Jenkins job if the quality gate is 'FAILED'
                    waitForQualityGate abortPipeline: true 
        }
            }
        }
        stage('Build Image') {
            steps {
                script {
                    withAWS(region:'us-east-1',credentials:'aws-creds') {
                        sh """
                        aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin ${ACC_ID}.dkr.ecr.us-east-1.amazonaws.com
                        docker build -t ${ACC_ID}.dkr.ecr.us-east-1.amazonaws.com/${PROJECT}/${COMPONENT}:${appVersion} .
                        docker images
                        docker push ${ACC_ID}.dkr.ecr.us-east-1.amazonaws.com/${PROJECT}/${COMPONENT}:${appVersion}
                        """
            }
        }
    }
    }
    }
    post {

        always
        {
            echo " I will always say Hello !!!"
            cleanWs()
        }
        success {
            echo 'Code is SUCCESS!!'
        }
        failure {
            echo 'code is Failure!!'
        }
        aborted {
            echo 'Pipeline is aborted'
        }
        
    }
    }

