@Library('jira-jenkins-shared-lib') _

pipeline {
    agent any
    environment {
//        JIRA_BASE_URL = 'http://192.168.0.6:8080'
//        JIRA_CREDENTIALS_ID = 'localhost-jira-admin'
//        APW_UNAVAILABLE_STATUS = 'Down'
//        APW_AVAILABLE_STATUS = 'Up'
        SLEEP_TIME = '5s'
        APW_APPLICATION = 'eCommerce'
        VERSION = readMavenPom().getVersion()
    }
    parameters {
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
                apwCheckEnvironmentStatus check: {
                    sh 'timeout 5 wget --retry-connrefused --tries=5 --waitretry=1 -q http://192.168.0.6:8180 -O /dev/null'
                }

                script {
                    def project = jira httpMode: 'GET', path: '/rest/api/2/project/10000'
                    echo project.toString()
                }

                apwChangeStatus status:'Deploy'

                withEnv(['JENKINS_NODE_COOKIE=dontKill']) {
                    sh "nohup java -jar -Dserver.port=${env.SERVER_PORT} target/*.jar &"
                }
                sh "sleep ${env.SLEEP_TIME}"

                apwUpdateEnvironment body:[
                        url: "http://192.168.0.6:${env.SERVER_PORT}",
                        attributes: [
                                OS: env.ENV_OS,
                                Owner: env.ENV_OWNER,
                                Database: env.ENV_DATABASE
                        ]
                ]

                apwSetDeployedVersion version:env.VERSION
                apwChangeStatus status:'Up'
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
                apwChangeStatus status:'Deploy'

                withEnv(['JENKINS_NODE_COOKIE=dontKill']) {
                    sh "nohup java -jar -Dserver.port=${env.SERVER_PORT} target/*.jar &"
                }
                sh "sleep ${env.SLEEP_TIME}"

                apwUpdateEnvironment body:[
                        url: "http://192.168.0.6:${env.SERVER_PORT}",
                        attributes: [
                                OS: env.ENV_OS,
                                Owner: env.ENV_OWNER,
                                Database: env.ENV_DATABASE
                        ]
                ]

                apwSetDeployedVersion version:env.VERSION
                apwChangeStatus status:'Up'
            }
        }
    }
}