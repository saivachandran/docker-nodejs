pipeline{

	agent any

	environment {
		DOCKERHUB_CREDENTIALS=credentials('dockerhub-auth')
	}

	stages {
	
	    stage('checkout') {
           steps {
             
            git branch: 'dev', url: 'https://github.com/saivachandran/docker-nodejs.git'
            
		   }
		}
		
		
	stage('SonarQube analysis') {
      def scannerHome = tool 'SonarScanner 4.0';
      withSonarQubeEnv('sonarcube') { // If you have configured more than one global server connection, you can specify its name
        sh "${scannerHome}/bin/sonar-scanner"
       }
	}
    
		stage('Build') {

			steps {
				sh 'docker build -t chandransaiva/nodeapp:latest .'
			}
		}

		stage('Login') {

			steps {
				sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
				
			}
		}

		stage('Push') {

			steps {
				sh 'docker push chandransaiva/nodeapp:latest'
			}
		}
		
		
		 stage('deploy application') {

			steps {
				sh 'docker run -p 49160:8080 -d chandransaiva/nodeapp:latest'
			}
		}
		
	}

	post {
		always {
			sh 'docker logout'
		}
	}

}
