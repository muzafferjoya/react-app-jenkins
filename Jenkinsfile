pipeline {
  agent {
    docker {
     image 'node:14.16.0'
     args '-p 3000:3000'
    }
  }
  environment {
    CI = 'true'
    HOME = '.'
    npm_config_cache = 'npm-cache'
  }
  stages {
    stage('Git Clone'){
      steps {
        git 'https://github.com/muzafferjoya/react-app-jenkins.git'
      }
    }
    stage('Install Packages') {
      steps {
        sh 'npm install'
      }
    }
    stage('Test and Build') {
      parallel {
        stage('Run Tests') {
          steps {
            sh 'npm run test'
          }
        }
        
      }
    }
    }
}
