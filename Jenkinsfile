pipeline {
  agent any  
  stages {
    stage('Git Cloning'){
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
            withAWS(region:'us-east-1',credentials:'aws-id') {
              
              s3Upload(bucket: 'muzaffar-react', workingDir:'build', includePathPattern:'**/*', excludePathPattern:'.git/*, **/node_modules/**');
            }
            mail(subject: 'Staging Build', body: 'New Deployment to Staging', to: 'muzaffar.khan@eroam.com')
          }
        }
        
      }
    }
  }
}
