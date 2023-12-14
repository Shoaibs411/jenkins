// reference for jenkins syntax - https://www.jenkins.io/doc/book/pipeline/syntax/  
pipeline{
    agent any
    
    environment{
        ENV_URL = "pipeline.global-variable.com"             // Pipeline based variable(Global Variable)
        SSH_CRED = credentials('SSH_CRED')
    }

    parameters {
        string(name: 'PERSON', defaultValue: 'John', description: 'Who should I say hello to?')
        text(name: 'BIOGRAPHY', defaultValue: 'Pro Snooker Player', description: 'Enter some information about the person')
    }

    options { 
        buildDiscarder(logRotator(numToKeepStr: '10')) 
        timeout(time: 59, unit: 'MINUTES')
        }

    tools {
        maven 'maven-390' 
    }

    triggers { pollSCM('*/1 * * * *') }

    stages{
        stage("Name of the Stage - 1") {
            steps {
                sh "mvn --version"
                sh "echo Step 1 of Stage 1"
                sh "echo Name of the variable is ${ENV_URL}"
                sh "sleep 150"
            }
        }

        stage("Demo on Parallel stage"){

        parallel {
            stage('Downloading A') {
                steps {
                    sh "echo Download A in progress"
                    sh "sleep 120" 

                }
            }
            stage('Downloading B') {
                steps {
                    sh "echo Download B in progress"
                    sh "sleep 120" 
                }
            }
            stage('Downloadin C') {
                steps {
                    sh "echo Download C in progress"
                    sh "sleep 120"
                }
            }
        }

        stage("Name of the Stage - 2") {
            environment{
                ENV_URL = "stage.task-level-variable.com"
            }
            steps {
                sh "echo Step 1 of Stage 2"
                sh "echo Name of the variable is ${ENV_URL}"
                sh "env"
            }
        }
        stage("Name of the Stage - 3") {
            steps {
                sh "echo Step 1 of Stage 3"
            }
        }
    }
}
}