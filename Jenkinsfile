@Library('jenkins-jira-sharedlib@master') _

pipeline {
    agent any
    environment {
        JIRA_CREDENTIALS = credentials('localhost-jira-admin')
        JIRA_URL = 'http://192.168.0.6:8080'
        JIRA_CREDENTIALS_ID = 'localhost-jira-admin'
        JIRA_VERSION = '8.0.2'
//        JIRA_CONFIG = [
//                url: 'http://192.168.0.6:8080',
//                credentialsId: 'localhost-jira-admin',
//                version: '8.0.2'
//        ]
    }
    stages {
        stage('Hello world') {
            steps {
                script {
                    echo 'Defining Jira configuration'

//                    def JIRA_CONFIG = [
//                            url: 'http://192.168.0.6:8080',
//                            credentialsId: 'localhost-jira-admin',
//                            version: '8.0.2'
//                    ]
//                    jira JIRA_CONFIG {
//                        httpMethod 'GET'
//                        path '/rest/api/2/project'
//                        body null
//                    }

//                    echo jiraConfig.url
//                    def projects = jiraFunction('GET', '/rest/api/2/project')
                    def projects = jira httpMode: 'GET', path: '/rest/api/2/project'
//                    jiraClosure {
//                        httpMode 'GET'
//                        path '/rest/api/2/project'
//                    }

                    echo projects.toString()

//                    currentBuild.result = hudson.model.Result.SUCCESS
                }
            }
        }
    }
}