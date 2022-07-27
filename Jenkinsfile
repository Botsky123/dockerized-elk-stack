pipeline {

	environment {
		registryCredentials = "nexus"
		imageName = "alpine"
		registry = "35.88.173.249:8085/"
	}
	agent any
	stages {

		stage('Building image') {
      			steps{
        			script {
          				dockerImage = docker.build imageName
        				}
     					 }
   			 }
		 stage('Scan') {
      			steps {
        			sh 'grype image --no-progress --exit-code 1 --severity HIGH,CRITICAL $imageName'
      				}
    			}  		
		
		stage('Uploading to Nexus') {
     			steps{  
         			script {
             				docker.withRegistry( 'http://'+registry, registryCredentials ) {
             				dockerImage.push('v5')
         				 }
       				 }
     			 }
   		 }
	
		stage('Remove Unused docker image') {
      			steps{
         			sh 'docker rmi -f $(docker image ls -a -q)'
				}
    		}
	}

	post {
		always {
			sh 'docker logout'
		}
	}
}
