pipeline {
  agent any
  stages {


    stage('Test') {
        steps {
            echo 'Running unit tests...'
            sh './gradlew test'
            junit 'build/test-results/test/TEST-Matrix.xml'
            echo 'Archiving artifacts...'
            archiveArtifacts 'build/test-results/**/*'
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
              sh './gradlew sonar'
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



     stage('Build') {
          steps {
            echo 'Generation .jar and Documentation...'
            sh './gradlew build'
            sh './gradlew javadoc'
            echo 'Archiving artifacts...'
            archiveArtifacts artifacts: 'build/libs/*.jar'
            archiveArtifacts artifacts: 'build/docs/javadoc/**'
            //junit(testResults: 'build/reports/tests/test', allowEmptyResults: true)
          }
     }

     stage('Deploy') {
        steps {
            echo "Deployment..."
            sh './gradlew publish'
        }
     }




    stage('Notify') {
        steps {
            echo "Notification..."
            notifyEvents message: 'Build is created with success', token: '1pz9Q2Y6iaCplPrOxw3Ixo4905PL70vc'
        }
    }
  }


  post {
    success {
        mail to: "jl_chikouche@esi.dz",
        subject: "Build Succeeded",
        body: "This is an email that informs that the new Build is deployed with success!"
    }
    failure {
        mail to: "jl_chikouche@esi.dz",
        subject: "Build failed",
        body: "This is an email that informs that the new Build is deployed with failure!"
    }
  }
}
