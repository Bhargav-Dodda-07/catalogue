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
        stage('Deploy') {

            // input {
            //      message "Should we continue?"
            //      ok "Yes, we should."
            //      submitter "alice,bob"
            //      parameters {
            //          string(name: 'PERSON', defaultValue: 'Mr Jenkins', description: 'Who should I say hello to?')
            //     }
            // }

            when { 
                expression { "$params.DEPLOY" == "true" }
            }

            steps {
                script {
                    sh """
                        echo "Deploying"
                    """
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