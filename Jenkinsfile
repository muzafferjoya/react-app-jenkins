pipeline {
  agent any  
  stages {

  	stage('Git clone'){
        git 'https://github.com/muzafferjoya/react-app-jenkins.git'
    }
    
    stage('Install Packages') {
      steps {
        sh 'npm install'
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
              
              s3Upload(bucket: 'muzaffar-react', workingDir:'build', includePathPattern:'**/*');
            }
            
          }
        }
        
      }
    }
