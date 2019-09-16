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
        appName = "TimeTable"
        teamName = "iOS"
    }
    stages {
        stage('Run Unit and UI Tests') {
            steps {
                script {
                    try {
                        slackSend(channel: "pipeline", message: "[${teamName}]${appName} - Job Started! :)", sendAsText: true)
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
    }
    post {
        success {
            slackSend(channel: "pipeline", message: "[${teamName}]${appName} - Success! :)", sendAsText: true)
        }
        failure {
            slackSend(channel: "pipeline",color: "danger", message: "[${teamName}]${appName} - Failed! :)", sendAsText: true)
        }
    }
}