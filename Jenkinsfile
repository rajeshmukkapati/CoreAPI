def DockerTag() {
	def tag = sh script: 'git rev-parse HEAD', returnStdout:true
	return tag
	}
pipeline {
  agent {label 'master'}
  environment {
      registry = "dmukkapa/coreapi"
      DOCKER_TAG=DockerTag()
    }
  options { 
      timestamps () 
      buildDiscarder logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '', daysToKeepStr: '10', numToKeepStr: '5')	
  // numToKeepStr - Max # of builds to keep
  // daysToKeepStr - Days to keep builds
  // artifactDaysToKeepStr - Days to keep artifacts
  // artifactNumToKeepStr - Max # of builds to keep with artifacts	  
  }	
    stages {
        stage('Git CheckOut') {
            steps {
                git credentialsId: '31aef066-7e35-44d1-b390-563f4194d83e', url: 'https://github.com/rajeshmukkapati/CoreAPI.git'
            }
        }
         stage('Building Docker image'){
            steps{
               sh 'cd CoreAPI/'
               sh 'docker build -t ${registry}:${DOCKER_TAG} .'
            }
        }
        stage('Push Docker Image'){
            steps{
            withCredentials([usernamePassword(credentialsId: 'Docker_UserName', passwordVariable: 'password', usernameVariable: 'UserName')]) {
            sh 'docker login -u ${UserName} -p ${password}'
            sh 'docker push ${registry}:${DOCKER_TAG}'
				    }
			    }
        }
        stage('Remove the docker image in local machine'){
           steps{
            sh 'docker rmi -f ${registry}:${DOCKER_TAG}'
			    }
        }
    }
}