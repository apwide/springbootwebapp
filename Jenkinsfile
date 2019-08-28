@Library('apwide-jenkins-shared-lib') _

pipeline {
    agent any
    environment {
        APW_JIRA_BASE_URL = 'http://192.168.0.6:8080'
        APW_JIRA_CREDENTIALS_ID = 'localhost-jira-admin'
        SLEEP_TIME = '5s'
        APW_APPLICATION = 'eCommerce'
        APW_UNAVAILABLE_STATUS = 'Down'
        APW_AVAILABLE_STATUS = 'Up'
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
//        stage('Build & Test') {
//            steps {
//                script {
//                    sh 'mvn clean install'
//                }
//            }
//        }
        stage('Deploy on Dev') {
            when {
                equals expected: 'Dev', actual: params.PROMOTE_TO_ENV
            }
            environment {
                APW_CATEGORY = 'Test2'
                APW_PERMISSION_SCHEME = 'Default Environment Permission Scheme'
//                APW_ENVIRONMENT_ID = '6'
                APW_BUILD_FAIL_ON_ERROR = false
                SERVER_PORT = 8089
                ENV_OS = 'Windows'
                ENV_OWNER = 'info@apwide.com'
                ENV_DATABASE = 'Oracle'
            }
            steps {
                apwCreateApplication application: 'Julien', buildFailOnError: false,
                    applicationSchemeId: 1,
                    body:[
                        versionPrefix: 'ECOM',
                        mappedProjectId: 10100
                    ]

                apwCreateEnvironmentAndCategory application:'Julien',
                    category:'Test3',
                    permissionsScheme:'Default Environment Permission Scheme',
                    body: [
                        url: 'http://www.google.fr',
                        attributes: [
                            Database: [
                                    value: 'Oracle'
                                    ]
                        ]
                    ]


                script {
                    def environments_json = apwGetEnvironment application:'Julien', category:'Tewst3'
                    echo environments_json
                }

//                apwUpdateApplication application: 'Julien',
//                        applicationSchemeId: 1,
//                        body:[
//                                versionPrefix: 'DADA',
//                                mappedProjectId: 10100
//                        ]

//                apwDeleteApplication application: 'Julien'


//                apwCreateEnvironmentCategory category: 'Test Cat'

//                apwUpdateEnvironmentCategory category: 'Test Cat', body: [
//                        order: 200
//                ]

//                apwDeleteEnvironmentCategory category: 'Test Cat'

//                apwCreateEnvironmentAndCategory application:'eCommerce',
//                    category:'Test2',
//                    permissionsScheme:'Default Environment Permission Scheme',
//                    body: [
//                        url: 'http://www.google.fr',
//                        attributes: [
//                            Database: [
//                                    value: 'Oracle'
//                                    ]
//                        ]
//                    ]

//                script {
//                }
//                    def envs = apwSearchEnvironments criteria:[
//                            applicationName:'eCommerce',
//                            statusName:['Up', 'Down'],
//                            '# servers':'4'
//                    ]
//                    echo envs.toString()
//
//                    apwCheckEnvironmentsStatus criteria:[
//                            applicationName:'eCommerce',
//                            statusName:['Up', 'Down'],
//                            '# servers':'4'
//                    ], checkStatus: { environment ->
//                        echo "youhou : ${environment.url}"
//                    }
//                }
//
//                apwWithEnvironments([
//                        applicationName:'eCommerce',
//                        statusName:['Up', 'Down'],
//                        '# servers':'4'
//                ]) { environment ->
//                    echo environment.url
//                }
//
//                apwSetEnvironmentStatus status:'Deploy'
//
////                withEnv(['JENKINS_NODE_COOKIE=dontKill']) {
////                    sh "nohup java -jar -Dserver.port=${env.SERVER_PORT} target/*.jar &"
////                }
////                sh "sleep ${env.SLEEP_TIME}"
//
//                apwUpdateEnvironment body:[
//                        url: "http://192.168.0.6:${env.SERVER_PORT}",
//                        attributes: [
//                                OS: env.ENV_OS,
//                                Owner: env.ENV_OWNER,
//                                Database: env.ENV_DATABASE,
//                                '# servers': '4'
//                        ]
//                ]
//
//                apwSetDeployedVersion version:env.VERSION
//                apwSetEnvironmentStatus status:'Up'
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
                apwSetEnvironmentStatus status:'Deploy'

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
                apwSetEnvironmentStatus status:'Up'
            }
        }
    }
}