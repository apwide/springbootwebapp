@Library('jenkins-jira-sharedlib@master') _

import groovy.json.JsonOutput

pipeline {
    agent any
    environment {
        JIRA_CREDENTIALS = credentials('localhost-jira-admin')
    }
    stages {
        stage('Hello world') {
            steps {
                script {
                    echo 'Defining Jira configuration'

                    def jiraConfig = [
                        url: 'http://192.168.0.6:8080',
                        credentialsId: 'localhost-jira-admin',
                        version: '8.0.2'
                    ]

//                    echo jiraConfig.url
                    def projects = jira jiraConfig, 'GET', '/rest/api/2/project'
                    echo projects.getClass().getName()
                    echo JsonOutput.toJson(projects as Map)

                    currentBuild.result = hudson.model.Result.SUCCESS
                }
            }
        }
    }
}