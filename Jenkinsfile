@Library('jenkins-jira-sharedlib@master') _

pipeline {
    agent any
    environment {
        JIRA_CREDENTIALS = credentials('localhost-jira-admin')
    }
    stages {
        stage('Hello world') {
            steps {
                script {
                    echo 'Hello EveryBody'
                    def jiraConfig = [
                        jiraCredentialsId = 'localhost-jira-admin',
                        jiraVersion = '7.13.2',
                        jiraUrl = 'http://192.168.0.6:8080'
                    ]
                    jira {
                        config : jiraConfig
//                        jiraCredentialsId = 'localhost-jira-admin'
//                        jiraVersion = '7.13.2'
//                        jiraUrl = 'http://192.168.0.6:8080'
                    }
                    currentBuilder.result = hudson.model.Result.SUCCESS
                }
            }
        }
    }
}