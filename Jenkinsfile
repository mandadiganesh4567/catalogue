pipeline {
    agent {
          node {
            label 'AGENT-1'
          }
    }
    environment {
        Course = 'jenkins'
        appVersion = ""
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
                    env.appVersion = packageJSON.version
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
        stage('Deploy') {
            steps {
                script {
                    sh """
                
                echo " Deploying"
                """
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
