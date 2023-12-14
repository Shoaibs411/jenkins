// reference for jenkins syntax - https://www.jenkins.io/doc/book/pipeline/syntax/  
pipeline{
    agent any
    environment{
        ENV_URL = "pipeline.global-variable.com"             // Pipeline based variable(Global Variable)
    }
    stages{
        stage("Name of the Stage - 1") {
            steps {
                sh "echo Step 1 of Stage 1"
                sh "echo Name of the variable is ${ENV_URL}"
            }
        }

        stage("Name of the Stage - 2") {
            environment{
                ENV_URL = "stage.task-level-variable.com"
            }
            steps {
                sh "echo Step 1 of Stage 2"
                sh "echo Name of the variable is ${ENV_URL}"
            }
        }
        stage("Name of the Stage - 3") {
            steps {
                sh "echo Step 1 of Stage 3"
                sh "echo Step 2 of Stage 3"
                sh "echo Step 3 of Stage 3"
            }
        }
    }
}