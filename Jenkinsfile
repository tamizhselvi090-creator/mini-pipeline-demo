pipeline {
  agent any
  tools {
    jdk 'jdk11'
    maven 'maven3'
  }

  stages {
    stage('Checkout') {
      steps {
        checkout scm
      }
    }

    stage('Build') {
      steps {
        bat 'mvn -B clean package'
      }
    }

    stage('Test') {
      steps {
        bat 'mvn -B test'
      }
    }

    stage('Deploy') {
      steps {
        bat '''
          if not exist "%WORKSPACE%\deployed" mkdir "%WORKSPACE%\deployed"
          copy target\*.jar "%WORKSPACE%\deployed\"
          dir "%WORKSPACE%\deployed"
        '''
      }
    }
  }

  post {
    always {
      junit 'target/surefire-reports/*.xml'
      archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
    }
  }
}
