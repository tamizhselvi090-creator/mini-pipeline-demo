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
        stage('Write and Read File') {
      steps {
        script {
          writeFile file: 'hello.txt', text: 'Hello from Jenkins!'
          def data = readFile('hello.txt')
          echo "The file contains: ${data}"
        }
      }
    }

    stage('JSON Example') {
      steps {
        script {
          writeJSON file: 'data.json', json: [name: 'MyApp', version: '1.0']
          def info = readJSON file: 'data.json'
          echo "App Name: ${info.name}, Version: ${info.version}"
        }
      }
    }

    stage('YAML Example') {
      steps {
        script {
          writeYaml file: 'app.yml', data: [service: 'backend', replicas: 2]
          def yamlData = readYaml file: 'app.yml'
          echo "YAML replicas: ${yamlData.replicas}"
        }
      }
    }

    stage('Zip Example') {
      steps {
        script {
          writeFile file: 'file1.txt', text: 'File 1'
          writeFile file: 'file2.txt', text: 'File 2'
          zip zipFile: 'files.zip', glob: '*.txt'
          echo "Created zip file successfully!"
        }
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
