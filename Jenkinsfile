//@Library('jenkins-jira-sharedlib@master') _
@Library('jenkins-jira-sharedlib@master')
import static com.apwide.jira.util.Utilities.*

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

                    def jiraConfig = jiraInstance {
                        url 'http://192.168.0.6:8080'
                        credentialsId 'jenkins-jira-admin'
                        version '8.0.2'
                    }

                    echo jiraConfig
//                    def projects = jira jiraConfig, 'GET', '/rest/api/2/project'
//                    echo projects

                    currentBuilder.result = hudson.model.Result.SUCCESS
                }
            }
        }
    }
}