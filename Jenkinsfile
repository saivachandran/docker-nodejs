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
      tools {
        sonarQube 'sonarcube-scanner 4.6.1.2450'
      }
      steps {
        withSonarQubeEnv('sonarcube-scanner') {
         sh "${scannerHome}/bin/sonar-scanner"
        }
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
