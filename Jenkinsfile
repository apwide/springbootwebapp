@Library("jenkins-jira-sharedlib@master") _

pipeline {
    agent any
    stages {
        stage("Hello world") {
            steps {
                script {
                    echo "Hello EveryBody"
                    jiraCredentials = credentials('localhost-jira-admin')
                    jira {
                        jiraVersion = "7.13.2"
                        jiraUser = jiraCredentials_USR
                        jiraPassword = jiraCredentials_PSW
                    }
                }
            }
        }
    }
}