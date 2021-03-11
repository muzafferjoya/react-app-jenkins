pipeline {
  agent {
    docker {
     image 'node:14.16.0'

     args '-p 8081:8081'

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
        stage('Create Build Artifacts') {
          steps {
            sh 'npm run build'
          }
        }
      }
    }

stage('Production') {
  steps {
    withAWS(region:'us-east-1',credentials:'aws-id') {
    s3Upload(bucket: 'muzaffar-prod', workingDir:'build', includePathPattern:'**/*', excludePathPattern:'.git/*, **/node_modules/**');
            }
          }
        }
    }
}
