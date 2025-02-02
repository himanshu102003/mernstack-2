pipeline {
  agent any
  tools {
    nodejs 'sonarnode'
  }
  environment {
    PATH = 'C:\\Windows\\System32'
    NODEJS_HOME = 'C:\\Program Files\\nodejs'
    SONAR_SCANNER_PATH = 'C:\\Users\\himan\\Downloads\\sonar-scanner-cli-6.2.1.4610-windows-x64\\sonar-scanner-6.2.1.4610-windows-x64\\bin'
  }
  stages {
    stage('Checkout') {
      steps {
        checkout scm
      }
    }

    stage('Install Dependencies') {
      steps {
        bat '''
        set PATH=%NODEJS_HOME%;%PATH%
        npm install
        '''
      }
    }

    stage('Lint') {
      steps {
        bat '''
        set PATH=%NODEJS_HOME%;%PATH%
        npm run lint
        '''
      }
    }

    stage('Build') {
      steps {
        bat '''
        set PATH=%NODEJS_HOME%;%PATH%
        npm run build
        '''
      }
    }

    stage('SonarQube Analysis') {
      environment {
        SONAR_TOKEN = credentials('sonarqube-token')  // Use Jenkins credentials for the token
      }
      steps {
        bat '''
        set PATH=%SONAR_SCANNER_PATH%;%PATH%
        where sonar-scanner || echo "SonarQube scanner not found. Please install it."
        sonar-scanner -Dsonar.projectKey=mernstack ^
        -Dsonar.sources=. ^
        -Dsonar.host.url=http://localhost:9000 ^
        -Dsonar.token=%SONAR_TOKEN%
        '''
      }
    }
  }

  post {
    success {
      echo 'Pipeline completed successfully'
    }
    failure {
      echo 'Pipeline failed'
    }
    always {
      echo 'This runs regardless of the result.'
    }
  }
}

