pipeline {
  agent any
  stages {
    stage("test"){
      steps{
      bat 'gradlew test'
      junit 'build/test-results/test/*.xml'
      cucumber buildStatus: 'UNSTABLE',
      reportTitle: 'My report',
      fileIncludePattern: 'target/report.json'
      }
    }
    stage('SonarQube analysis') {
      steps{
       withSonarQubeEnv("sonar") { // Will pick the global server connection you have configured
         bat 'gradlew sonarqube' }
      }
   
    }
   
    stage("Quality Gate") {
            steps {
                timeout(time: 1, unit: 'HOURS') {
                    // Parameter indicates whether to set pipeline to UNSTABLE if Quality Gate fails
                    // true = set pipeline to UNSTABLE, false = don't
                    waitForQualityGate abortPipeline: true
                }
            }
        }
     stage("Build") {
            steps {
                bat 'gradlew build'
                bat 'gradlew javadoc'
                archiveArtifacts 'build/libs/*.jar'
                archiveArtifacts 'build/docs/'
            }
        }
     stage("deploy") {
            steps {
                bat 'gradlew publish'

            }
        }
       
    
    }
   
}

}
