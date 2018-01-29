pipeline {
  agent {
    label "prod"
  }
  options {
    buildDiscarder(logRotator(numToKeepStr: '2'))
    disableConcurrentBuilds()
  }
  stages {
    stage("notify") {
      steps {
        slackSend(
          color: "info",
          message: "${env.JOB_NAME} started: ${env.RUN_DISPLAY_URL}"
        )
      }
    }
    stage("setup") {
      when {
        branch "master"
      }
      steps {
        sh "docker network create --driver overlay servicebus || echo 'Network creation failed. It probably already exists.'"
      }
    }
    stage("deploy") {
      when {
        branch "master"
      }
      environment {
        RMQ1_DOMAIN = "${env.rmq1Domain}"
        ERLANG_COOKIE = "${env.rmqErlangCookie}"
        RMQ_USER = "${env.rmqUser}"
        RMQ_PASSWORD = "${env.rmqPassword}"
      }
      steps {
        script {
          if (env.rmq1Domain && env.erlangCookie) {
            sh "docker stack deploy -c stack.yml servicebus"
          } else {
            sh 'echo "ERROR: env.rmq1Domain and env.erlangCookie are required." && exit 1'
          }
        }
      }
    }
  }
  post {
    always {
      sh "docker system prune -f"
    }
    failure {
      slackSend(
        color: "danger",
        message: "${env.JOB_NAME} failed: ${env.RUN_DISPLAY_URL}"
      )
    }
    success {
      slackSend(
        color: "success",
        message: "${env.JOB_NAME} succeeded: ${env.RUN_DISPLAY_URL}"
      )
    }
  }
}