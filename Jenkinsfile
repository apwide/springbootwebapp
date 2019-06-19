@Library("jenkins-jira-sharedlib@master") _

pipeline {
    agent any
    stages {
        stage("Hello world") {
            steps {
                script {
                    echo "Hello EveryBody"
                    jira {
                        jiraVersion = "7.13.2"
                    }
                }
            }
        }
    }
}