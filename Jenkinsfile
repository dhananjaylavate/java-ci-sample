pipeline {
  agent any
  tools {
    jdk 'jdk11'                 // match name in Global Tool Configuration
    maven 'maven-3.8.x'         // match name there
  }
  environment {
    MAVEN_OPTS = '-Xmx1024m'
  }
  stages {
    stage('Checkout') {
      steps {
        // git step uses credential ID added to Jenkins credentials
       git branch: 'main', url: 'https://github.com/dhananjaylavate/java-ci-sample.git', credentialsId: 'github-access-token'
      }
    }

 stage('Build & Test') {
  steps {
    script {
      if (isUnix()) {
        sh 'mvn clean install'
      } else {
        // Use MAVEN_HOME directly and expand properly
        bat "\"%MAVEN_HOME%\\bin\\mvn.cmd\" clean install"
        
      }
    }
  }
}


    stage('Publish Results & Archive') {
      steps {
        // Publish JUnit XML reports (adjust path if needed)
        junit '**/target/surefire-reports/*.xml'
        // Archive artifacts (jar)
        archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
      }
    }
  }

  post {
    always {
      // keep console compact and clean workspace
      echo "Cleaning workspace"
      cleanWs()
    }
    success {
      echo "Build succeeded"
    }
    failure {
      echo "Build failed"
    }
  }
}
