env.DOCKER_REGISTRY = 'raxer'
env.DOCKER_IMAGE_NAME = 'landing'

node('master') {
	stage('HelloWorld') {
	  sh "whoami"
	}
	stage('Get File Github') {
	  sh "rm -rf *"
	  sh "git clone https://github.com/rafifauz/StaticWeb-Jenkins.git"
	  sh "mv StaticWeb-Jenkins/* . && rm -rf StaticWeb-Jenkins"
	}
	stage('Build Docker Image') {
	  sh "docker build -t $DOCKER_REGISTRY/$DOCKER_IMAGE_NAME:${BUILD_NUMBER} ."
	}  
	stage('Push Docker Image') {
	  sh "docker push $DOCKER_REGISTRY/$DOCKER_IMAGE_NAME:${BUILD_NUMBER}"
	}
	stage('Deploy Image to Kubernetes') {
	 sh """ sed -i 's;raxer/landing;raxer/landing:${BUILD_NUMBER};g' LandingPage-ingress-SSL.yml """
	 sh "kubectl apply -f LandingPage-ingress-SSL.yml"
	}
	stage('Delete Image') {
	  sh "docker rmi $DOCKER_REGISTRY/$DOCKER_IMAGE_NAME:${BUILD_NUMBER}"
	}
	stage('Waint until success') {
	  cleanWs()
	}
}
