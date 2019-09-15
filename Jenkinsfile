pipeline {
    agent {
        label 'mac-mini'
    }
    environment {
        CI = 'true'
    }
    stages {
        stage('GitSCM') {
            steps {
                checkout([
                    $class: 'GitSCM',
                    branches: [[name: 'master']],
                    doGenerateSubmoduleConfigurations: false,
                    extensions: [], submoduleCfg: [],
                    userRemoteConfigs: [[
                        name: 'github',
                        url: 'https://github.com/mmorejon/time-table.git'
                    ]]
                ])
            }
        }
        stage('Run Unit and UI Tests') {
            steps {
                script {
                    try {
                      sh "fastlane runTests" 
                    } catch(exc) {
                      error('There are failed tests.')
                    }                    
                }
            }
        }
        stage('Build application for beta') {
            steps {
                sh "fastlane beta"
            }
        }

        stage('Slark Noti') {
            steps {
                slackSend(channel: "pipeline", message: "Success! :)", sendAsText: true)
            }
        }
    }
}