@Library("jenkins-jira-sharedlib@master") _

pipeline {
    agent any
    environment {
        JIRA_CREDENTIALS = credentials('localhost-jira-admin')
    }
    stages {
        stage("Hello world") {
            steps {
                script {
                    echo "Hello EveryBody"
                    jira {
                        jiraBuildFailOnError = false
                        jiraCredentialsId = 'localhost-jira-admin'
                        jiraVersion = "7.13.2"
                    }
                }
            }
        }
    }
}