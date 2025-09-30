pipeline {
  agent any
  tools {
    jdk 'jdk11'
    maven 'maven-3.8.x'
  }
  stages {
    stage('Checkout') {
      steps {
        git url: 'https://github.com/dhananjaylavate/java-ci-sample.git', credentialsId: 'github-creds-practise'
      }
    }
    stage('Build & Test') {
      steps {
        sh 'mvn -B clean verify'
      }
    }
    stage('Publish Results') {
      steps {
        junit '**/target/surefire-reports/*.xml'
        archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
      }
    }
  }
  post {
    always {
      cleanWs()
    }
  }
}