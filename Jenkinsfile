@Library('jenkins-jira-sharedlib@master') _

pipeline {
    agent any
    environment {
        JIRA_CREDENTIALS = credentials('localhost-jira-admin')
        JIRA_CONFIG = [
                url: 'http://192.168.0.6:8080',
                credentialsId: 'localhost-jira-admin',
                version: '8.0.2'
        ]
    }
    stages {
        stage('Hello world') {
            steps {
                script {
                    echo 'Defining Jira configuration'

//                    jira JIRA_CONFIG {
//                        httpMethod 'GET'
//                        path '/rest/api/2/project'
//                        body null
//                    }

//                    echo jiraConfig.url
                    def projects = jira config: JIRA_CONFIG, httpMode: 'GET', path: '/rest/api/2/project'
                    echo projects.toString()

//                    currentBuild.result = hudson.model.Result.SUCCESS
                }
            }
        }
    }
}