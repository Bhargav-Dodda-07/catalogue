pipeline {
    // These are pre-Build sections
    agent {
        node {
            label 'AGENT-1'
        }
    }

    environment {
        COURSE = "Jenkins"
        appVersion = ""
        ACC_ID = "240316737903"
        PROJECT = "roboshop"
        COMPONENT = "catalogue"
    }

     options {
        timeout(time: 10, unit: 'MINUTES')
        disableConcurrentBuilds() 
    }

    
    stages {
        // This is Build Section
        stage('Read Version') {
            steps {
                script {
                    def packageJSON = readJSON file: 'package.json'
                    appVersion = packageJSON.version
                    echo "app version is: ${appVersion}"
                }
            }
        }
        stage('Install Dependencies') {
            steps {
                script {
                    sh """
                        npm install
                    """
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

    // These are post-Build sections
    post {
        always {
            echo 'I will always say Hello again!'
            cleanWs()
        }

        success {
            echo 'I will run if succeeded'
        }

        failure {
            echo 'I will run if fails'
        }

        aborted {
            echo 'pipeline is aborted'
        }
    }
    
}