pipeline{

	agent any

	environment {
		DOCKERHUB_CREDENTIALS=credentials('dockerhub-auth')
	}

	stages {

		stage('Build') {

			steps {
				sh 'docker build -t chandransaiva/nodeapp:latest .'
			}
		}

		stage('Login') {

			steps {
				sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
				
				// sh docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD
				
				// sh  docker login -u $DOCKERHUB_CREDENTIALS_USR -p $DOCKERHUB_CREDENTIALS_PSW '
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
