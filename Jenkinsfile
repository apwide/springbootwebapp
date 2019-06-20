@Library('jenkins-jira-sharedlib@master') _

pipeline {
    agent any
    environment {
        JIRA_CREDENTIALS = credentials('localhost-jira-admin')
        JIRA_BASE_URL = 'http://192.168.0.6:8080'
        JIRA_CREDENTIALS_ID = 'localhost-jira-admin'
        JIRA_VERSION = '8.0.2'
    }
    stages {
        stage('Hello world') {
            steps {
                script {
                    echo 'Defining Jira configuration'

                    def JIRA_CONFIG = [
                            baseUrl: 'http://192.168.0.6:8080',
                            credentialsId: 'localhost-jira-admin',
                            version: '8.0.2'
                    ]

                    def projects = jira httpMode: 'GET', path: '/rest/api/2/project'
                    echo projects.toString()

                    def projects2 = jira {
                        httpMode = 'GET'
                        path = '/rest/api/2/project'
                    }
                    echo projects2.toString()

                    def project = jiraGetProject(JIRA_CONFIG) {
                        id = '10000'
                    }

                    echo project.toString()
                }
            }
        }
    }
}