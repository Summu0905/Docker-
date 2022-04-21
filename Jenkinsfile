node {
  
  stage('Checkout Source Code') {
    checkout scm
  }

  stage('Create Docker Image') {
    docker.build("docker_image:${env.BUILD_NUMBER}")
  }

  stage ('Run Application') {
    try {
      // Stop existing Container
      sh 'docker rm docker_container -f'
      // Start database container here
      sh "docker run -d --name docker_container docker_image:${env.BUILD_NUMBER}"
    } 
	catch (error) {
    } finally {
      // Stop and remove database container here
      
    }
  }
	//sudo usermod -a -G docker jenkins
	
stage('Deploy Image') {
	steps{
		script {
		docker.withRegistry( '', registryCredential ) {
		dockerImage.push("$BUILD_NUMBER")
		dockerImage.push('latest')
		}
		}
	}
}
stage('Remove Unused docker image') {
		steps{
		sh "docker rmi $imagename:$BUILD_NUMBER"
		sh "docker rmi $imagename:latest"
	}
	}
}
 
  
   
 }
