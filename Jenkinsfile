pipeline {
  agent any
  stages {


    stage('Test') {
        steps {
            echo 'Running unit tests...'
            bat 'gradlew test'
            junit allowEmptyResults: true, testResults: '**/test-results/test/*.xml'
            echo 'Archiving artifacts...'
            archiveArtifacts '/**/*'
            echo 'Generation Cucumber report'
            cucumber buildStatus: 'UNSTABLE',
                       reportTitle: 'My report',
                       fileIncludePattern: 'target/report.json',
                       trendsLimit: 10
        }
    }

    stage('Code Analysis') {
            steps {
                echo 'Running gradle sonar'
                withSonarQubeEnv('sonar') {
                  bat 'gradlew sonar'
                }
            }
        }



        stage("Code Quality") {
            steps {
                timeout(time: 1, unit: 'HOURS') {
                   waitForQualityGate abortPipeline: true
                }
            }
        }


  }

}
