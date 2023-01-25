pipeline {
  agent any
  stages {


    stage('Test') {
        steps {
            echo 'Running unit tests...'
            bat 'gradlew test'
            unit allowEmptyResults: true, testResults: '**/test-results/test/*.xml'
            echo 'Archiving artifacts...'
            archiveArtifacts 'build/test-results/**/*'
            echo 'Generation Cucumber report'
            cucumber buildStatus: 'UNSTABLE',
                       reportTitle: 'My report',
                       fileIncludePattern: 'target/report.json',
                       trendsLimit: 10
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
