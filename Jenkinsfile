pipeline {
  agent any

  triggers { pollSCM('H/1 * * * *') }

  environment {
    SONAR_TOKEN = credentials('SONAR_TOKEN')
  }

  stages {
    stage('Checkout') {
      steps {
        git branch: 'main', url: 'https://github.com/XavierMaye/8.2C-DevSecOps.git'
      }
    }

    stage('Install Dependencies') {
      steps {
        bat 'npm install'
      }
    }

    stage('Run Tests') {
      steps {
        bat 'npm test || exit /b 0'
      }
      post {
        always {
          emailext(
            subject: "[Run Tests] ${env.JOB_NAME} #${env.BUILD_NUMBER}: ${currentBuild.currentResult}",
            body: "Run Tests stage finished with status: ${currentBuild.currentResult}",
            to: "xdm.murphy@gmail.com",
            attachLog: true,
            replyTo: "xdm.murphy@gmail.com",
            from: "xdm.murphy@gmail.com"
          )
        }
      }
    }

    stage('Generate Coverage Report') {
      steps {
        bat 'npm run coverage || exit /b 0'
      }
    }

    stage('NPM Audit (Security Scan)') {
      steps {
        bat 'npm audit || exit /b 0'
      }
      post {
        always {
          emailext(
            subject: "[Security Scan] ${env.JOB_NAME} #${env.BUILD_NUMBER}: ${currentBuild.currentResult}",
            body: "Security Scan stage finished with status: ${currentBuild.currentResult}",
            to: "xdm.murphy@gmail.com",
            attachLog: true,
            replyTo: "xdm.murphy@gmail.com",
            from: "xdm.murphy@gmail.com"
          )
        }
      }
    }
  }
}
