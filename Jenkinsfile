env.DOCKER_REGISTRY = 'raxer'
env.DOCKER_IMAGE_NAME = 'pesbuk2'

node('master') {
	stage('HelloWorld') {
	  sh "whoami"
	}
	stage('Get File Github') {
	  sh "rm -rf *"
	  sh "git clone https://github.com/rafifauz/Pesbuk-Jenkins.git"
	  sh "mv Pesbuk-Jenkins/* . && rm -rf Pesbuk-Jenkins"
	}
	stage('Build Docker Image') {
	  sh "docker build -t $DOCKER_REGISTRY/$DOCKER_IMAGE_NAME:${BUILD_NUMBER} ."
	}  
	stage('Push Docker Image') {
	  sh "docker push $DOCKER_REGISTRY/$DOCKER_IMAGE_NAME:${BUILD_NUMBER}"
	}
	stage('Deploy Image to Kubernetes') {
	 sh """ sed -i 's;raxer/pesbuk2;raxer/pesbuk2:${BUILD_NUMBER};g' secret-pesbuk-ingress-SSL.yml """
	 sh "kubectl apply -f secret-pesbuk-ingress-SSL.yml"
	}
	stage('Delete Image') {
	  sh "docker rmi $DOCKER_REGISTRY/$DOCKER_IMAGE_NAME:${BUILD_NUMBER}"
	}
	stage('Waint until success') {
	  cleanWs()
	}
}
