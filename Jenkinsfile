import java.time.LocalDate

@Library('jenkins-jira-sharedlib@master') _

pipeline {
    agent any
    environment {
        JIRA_CREDENTIALS = credentials('localhost-jira-admin')
        JIRA_BASE_URL = 'http://192.168.0.6:8080'
        JIRA_CREDENTIALS_ID = 'localhost-jira-admin'
        JIRA_VERSION = '8.0.2'
        SLEEP_TIME = '5s'
        APW_APPLICATION = 'eCommerce'
        VERSION = readMavenPom().getVersion()
    }
    parameters {
        booleanParam(name: 'RELEASE', defaultValue: false, description: 'Should a release be created and published ?')
        choice(name: 'PROMOTE_TO_ENV', choices: ['Dev', 'Demo'], description: 'Should the output be promoted to an environment ?')
    }
    tools {
        maven 'mvn3'
        jdk 'jdk8'
    }
    stages {
        stage('Build & Test') {
            steps {
                script {
                    sh 'mvn clean install'
                }
            }
        }
        stage('Deploy on Dev') {
            when {
                equals expected: 'Dev', actual: params.PROMOTE_TO_ENV
            }
            environment {
                APW_CATEGORY = 'Dev'
                APW_ENVIRONMENT_ID = '6'
                SERVER_PORT = 8089
                ENV_OS = 'Windows'
                ENV_OWNER = 'info@apwide.com'
                ENV_DATABASE = 'Oracle'
            }
            steps {
                script {
                    def project = jira httpMode: 'GET', path: '/rest/api/2/project/10000'
                    echo project.toString()
                }

                apwStatusChanged status:'Deploy'

                withEnv(['JENKINS_NODE_COOKIE=dontKill']) {
                    sh "nohup java -jar -Dserver.port=${env.SERVER_PORT} target/*.jar &"
                }
                sh "sleep ${env.SLEEP_TIME}"

                apwEnvironmentUpdated body:[
                        url: "http://192.168.0.6:${env.SERVER_PORT}",
                        attributes: [
                                OS: env.ENV_OS,
                                Owner: env.ENV_OWNER,
                                Database: env.ENV_DATABASE
                        ]
                ]

                apwDeployedVersion version:env.VERSION
                apwStatusChanged status:'Up'
            }
        }
        stage('Release') {
            when {
                expression { return params.RELEASE }
            }
            steps {
                sh 'mvn release:prepare'
                sh 'mvn release:perform'
            }
        }
        stage('Deploy on Demo') {
            when {
                equals expected: 'Demo', actual: params.PROMOTE_TO_ENV
            }
            environment {
                APW_CATEGORY = 'Demo'
                APW_ENVIRONMENT_ID = '7'
                SERVER_PORT = 8090
                ENV_OS = 'Ubuntu'
                ENV_OWNER = 'support@apwide.com'
                ENV_DATABASE = 'Oracle'
            }
            steps {
                apwStatusChanged status:'Deploy'

                withEnv(['JENKINS_NODE_COOKIE=dontKill']) {
                    sh "nohup java -jar -Dserver.port=${env.SERVER_PORT} target/*.jar &"
                }
                sh "sleep ${env.SLEEP_TIME}"

                apwEnvironmentUpdated body:[
                        url: "http://192.168.0.6:${env.SERVER_PORT}",
                        attributes: [
                                OS: env.ENV_OS,
                                Owner: env.ENV_OWNER,
                                Database: env.ENV_DATABASE
                        ]
                ]

                apwDeployedVersion version:env.VERSION
                apwStatusChanged status:'Up'
            }
        }
    }
}