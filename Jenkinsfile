pipeline{

	agent any

	environment {
		DOCKERHUB_CREDENTIALS=credentials('dockerhub-auth')
	}

	stages {
	
	    stage('checkout') {
           steps {
             
            git branch: 'dev', url: 'https://github.com/saivachandran/docker-nodejs.git'

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
	}

	post {
		always {
			sh 'docker logout'
		}
	}

}
