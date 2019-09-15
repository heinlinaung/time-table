node('mac-mini') {
    // stages {
        withEnv([PATH = "$HOME/.fastlane/bin:" +
                "$HOME/.rvm/gems/ruby-2.5.3/bin:" +
                "$HOME/.rvm/gems/ruby-2.5.3@global/bin:" +
                "$HOME/.rvm/rubies/ruby-2.5.3/bin:" +
                "/usr/local/bin:" +
                "$PATH"
             LC_ALL = "en_US.UTF-8"
             LANG = "en_US.UTF-8"
            ]) {
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
              // steps {
                // script {
                  try {
                    sh "fastlane runTests" 
                  } catch(exc) {
                    error('There are failed tests.')
                  }
                // }
              // }
            }

            stage('Build application for beta') {
              // steps {
                sh "fastlane beta"
              // }
            }

            stage('Inform Slack for success') {
                // steps {
                    slackSend(channel: "pipeline", message: "Success! :)", sendAsText: true)
                // }
            }    
        }
        
    // }

    post {
        failure {
            slackSend(channel: "pipeline",color: "danger", message: "Failed! :)", sendAsText: true)
        }
        unstable {
            slackSend(channel: "pipeline",color: "danger", message: "Job is unstable. Unstable means test failure, code violation etc! :)", sendAsText: true)
        }
    }
}
