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
        // Create deployed folder (PowerShell handles paths safely)
        bat 'powershell -Command "New-Item -ItemType Directory -Path \\\"$Env:WORKSPACE\\\\deployed\\\" -Force"'

        // Copy jar(s) to deployed folder
        bat 'powershell -Command "Copy-Item -Path target\\\\*.jar -Destination \\\"$Env:WORKSPACE\\\\deployed\\\" -Force"'

        // List deployed folder contents
        bat 'powershell -Command "Get-ChildItem -Path \\\"$Env:WORKSPACE\\\\deployed\\\" | Format-Table -AutoSize"'
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
