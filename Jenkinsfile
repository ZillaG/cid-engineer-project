pipeline {
  agent any
  environment {
    PATH = "/usr/local/bin:$PATH"
    DOCKER_URL = "092258231695.dkr.ecr.us-east-1.amazonaws.com"
    DOCKER_CREDS = "ecr:us-east-1:test-user-aws"
    DOCKER_IMG = "node-app"
  }
  stages {
    stage("Build app") {
      steps {
        sh "npm install"
      }
    }
    stage("Test app") {
      steps {
        sh """
          pm2 start ./bin/www
          npm test
          pm2 stop ./bin/www
        """
      }
    }
    stage("Build and push Docker image") {
      steps {
        script {
          // configure registry
          docker.withRegistry("https://$DOCKER_URL", DOCKER_CREDS) {
            //build image
            def customImage = docker.build("$DOCKER_URL/$DOCKER_IMG:latest")
            //push image
            customImage.push()
          }
        }
      }
    }
  }
  post {
    // Deployment job only runs if above stages all pass
    success {
      build job: "deploy-sandbox",
            wait: false
    }
  }
}
