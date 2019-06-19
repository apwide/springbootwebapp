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
                    jiraCredentials = credentials('localhost-jira-admin')
                    jira {
                        jiraVersion = "7.13.2"
                        jiraUser = JIRA_CREDENTIALS_USR
                        jiraPassword = JIRA_CREDENTIALS_PSW
                    }
                }
            }
        }
    }
}