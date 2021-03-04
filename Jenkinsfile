node {
    
    stage('Git clone'){
        git 'https://github.com/muzafferjoya/react-app-jenkins.git'
    }

    stage('Install Dependencies'){
	   sh 'npm install'
}

	

	stage('Building'){
		sh 'npm run build'
	}

    
    stage('Deploy To S3 Bucket') {

        dir('/var/lib/jenkins/workspace/react-jenkins'){

        pwd(); 

        withAWS(region:'us-east-1',credentials:'aws-id') {

        def identity=awsIdentity();
		
	s3Upload(bucket:"muzaffar-react", workingDir:'build', includePathPattern:'**/*', excludePathPattern:'.git/*, node_modules/*');
	s3Delete(bucket: 'muzaffar-react', path:'**/*');
	s3Upload(bucket:"muzaffar-react", workingDir:'build', includePathPattern:'**/*', excludePathPattern:'.git/*, node_modules/*');
	
                 
        }

        };

	}
}
