pipeline {
  agent any
  stages {
    stage("Trigger test-infra job") {
      steps {
        build job: "test-infra",
              wait: true
      }
    }
  }
}
