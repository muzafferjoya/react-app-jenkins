pipeline {
  agent {
    docker {
      image 'node:14.16.0'
      args '-p 20001-20100:3000'
    }
  }
  environment {
    CI = 'true'
    HOME = '.'
    npm_config_cache = 'npm-cache'
  }
  stages {
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
        stage('Create Build Artifacts') {
          steps {
            sh 'npm run build'
          }
        }
      }
    }
    stage('Deployment') {
      parallel {
        stage('Staging') {
          when {
            branch 'staging'
          }
          steps {
            withAWS(region:'us-east-1',credentials:'muzaffar-aws-id') {
              s3Delete(bucket: 'muzaffar-khan/staging', path:'**/*')
              s3Upload(bucket:'muzaffar-khan/staging', workingDir:'build', includePathPattern:'**/*', excludePathPattern:'.git/*, **/node_modules/**');
            }
            
          }
        }
        stage('Production') {
          when {
            branch 'master'
          }
          steps {
            withAWS(region:'us-east-1',credentials:'muzaffar-aws-id') {
              s3Delete(bucket: 'muzaffar-khan/master', path:'**/*')
              s3Upload(bucket:'muzaffar-khan/master', workingDir:'build', includePathPattern:'**/*', excludePathPattern:'.git/*, **/node_modules/**');
            }
            
          }
        }
      }
    }
  }
}
