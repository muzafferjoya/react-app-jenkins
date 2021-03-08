node {
    
    stage('Git clone'){
        git 'https://github.com/muzafferjoya/react-app-jenkins.git'
    }

    stage('Install Dependencies'){
	   sh 'npm install'
}

	stage('Test'){
		sh 'npm run test'
	}

	stage('Building'){
		sh 'npm run build'
	}

    
    stage('AWS Credentials Check') {

        dir('/var/lib/jenkins/workspace/job-1'){

        pwd(); 

        withAWS(region:'us-east-1',credentials:'aws-id') {

        def identity=awsIdentity();
		
	    	s3Upload(bucket:"muzaffar-react", workingDir:'build', includePathPattern:'**/*', excludePathPattern:'.git/*, node_modules/*');
                 
        }

        };

	}
}
