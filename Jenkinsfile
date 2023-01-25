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



        stage("Quality Gate") {
                    steps {
                        timeout(time: 1, unit: 'HOURS') {
                            // Parameter indicates whether to set pipeline to UNSTABLE if Quality Gate fails
                            // true = set pipeline to UNSTABLE, false = don't
                            //waitForQualityGate abortPipeline: true
                        }
                    }
                }

         stage("Build") {
                    steps {
                        bat 'gradlew build'
                        bat 'gradlew javadoc'
                        archiveArtifacts 'build/libs/*.jar'
                        archiveArtifacts '**'
                    }
                }

         stage("deploy") {
                     steps {
                         bat 'gradlew publish'
                      }
                 }

         stage('Notify') {
                 steps {
                     echo "Notification..."
                     notifyEvents message: 'Build is created with success', token: '1pz9Q2Y6iaCplPrOxw3Ixo4905PL70vc'
                 }
             }


  }


}
