node('mac-mini') {
    stages {
        stage('Checkout/Build/Test') {

            // Checkout files.
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

            // Build and Test
            // sh 'xcodebuild -scheme "TimeTable" -configuration "Debug" build test -destination "platform=iOS Simulator,name=iPhone 6,OS=12.1" -enableCodeCoverage YES | /usr/local/bin/xcpretty -r junit'

            // Publish test restults.
            // step([$class: 'JUnitResultArchiver', allowEmptyResults: true, testResults: 'build/reports/junit.xml'])
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

        stage('Inform Slack for success') {
            steps {
                slackSend(channel: "pipeline", message: "Success! :)", sendAsText: true)
            }
        }
    }

    post {
        failure {
            slackSend(channel: "pipeline",color: "danger", message: "Failed! :)", sendAsText: true)
        }
        unstable {
            slackSend(channel: "pipeline",color: "danger", message: "Job is unstable. Unstable means test failure, code violation etc! :)", sendAsText: true)
        }
    }
}
