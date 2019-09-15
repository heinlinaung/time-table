pipeline {
    agent {
        label 'mac-mini'
    }
    environment {
        // Fastlane Environment Variables
        PATH = "$HOME/.fastlane/bin:" +
                "$HOME/.rvm/gems/ruby-2.5.3/bin:" +
                "$HOME/.rvm/gems/ruby-2.5.3@global/bin:" +
                "$HOME/.rvm/rubies/ruby-2.5.3/bin:" +
                "/usr/local/bin:" +
                "$PATH"
        LC_ALL = "en_US.UTF-8"
        LANG = "en_US.UTF-8"
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