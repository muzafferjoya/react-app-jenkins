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
    stage('Testing'){
      steps {
        sh 'npm run test'
      }
    }
    stage('Build') {    
        
        stage('Create Build Artifacts') {
          steps {
            sh 'npm run build'
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
            s3Upload(bucket:"muzaffar-khan/staging", workingDir:'build', includePathPattern:'**/*', excludePathPattern:'.git/*, **/node_modules/**');
			s3Delete(bucket: 'muzaffar-khan/staging', path:'**/*');
			s3Upload(bucket:"muzaffar-khan/staging", workingDir:'build', includePathPattern:'**/*', excludePathPattern:'.git/*, **/node_modules/**');
            }
            
          }
        }
        stage('Production') {
          when {
            branch 'master'
          }
          steps {
            withAWS(region:'us-east-1',credentials:'muzaffar-aws-id') {
            s3Upload(bucket:"muzaffar-khan/staging", workingDir:'build', includePathPattern:'**/*', excludePathPattern:'.git/*, **/node_modules/**');
			s3Delete(bucket: 'muzaffar-khan/staging', path:'**/*');
			s3Upload(bucket:"muzaffar-khan/staging", workingDir:'build', includePathPattern:'**/*', excludePathPattern:'.git/*, **/node_modules/**');
            }
            
          }
        }
      }
    }
  }
}
