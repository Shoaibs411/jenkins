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
        //booleanParam(name: 'TOGGLE', defaultValue: true, description: 'Toggle this value')
        //choice(name: 'CHOICE', choices: ['One', 'Two', 'Three'], description: 'Pick something')
        //password(name: 'PASSWORD', defaultValue: 'SECRET', description: 'Enter a password')
    }

    // triggers {cron('*/1 * * * *')}

    options { 
        buildDiscarder(logRotator(numToKeepStr: '10')) 
        timeout(time: 1, unit: 'MINUTES')
        }

    triggers { pollSCM('*/1 * * * *') }

    stages{
        stage("Name of the Stage - 1") {
            steps {
                sh "mvn"
                sh "echo Step 1 of Stage 1"
                sh "echo Name of the variable is ${ENV_URL}"
                sh "sleep 150"
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