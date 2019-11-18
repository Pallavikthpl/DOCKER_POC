pipeline {
    agent any
	environment
	{
	ant_build = "C:\\Users\\PallaviKathpalia\\IBM\\IIBT10\\workspace_New\\IIB_Build_Process\\HttpReqReply\\Build\\build.xml"
	registry = "pallavikthpl/iibmq_poc1"
    	registryCredential = ‘dockerhub’
	dockerImage = ''
	}
    stages {
	    
        stage('Checkout') { 
            
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[url: 'https://github.com/Pallavikthpl/DOCKER_POC.git']]])
            }
        }
        
        
	  stage('Build') {
		steps
		{
		withAnt(installation: 'apache-ant-1.9.14', jdk: 'jdk-12.0.1'){
		// some block
		bat "ant -f ${ant_build}"
		}
		}
	}
	    stage('upload') {
		steps {
		script {
			def server = Artifactory.server 'JfrogArtifactory'
			def uploadSpec = """{
			"files": [{
			"pattern": "C:/Users/PallaviKathpalia/JenkinsUCD/",
			"target": "jenkins"
			}]
			}"""
 
			server.upload(uploadSpec)
			}
		}
		}
	    
        stage('Build Docker Image') {
                
            steps {
		    script{
                	dockerImage = docker.build registry + ":BUILD_NUMBER"
            }
	    }
        }
	    stage('Deploy Image') {
      steps{
        script {
          docker.withRegistry( '', registryCredential ) {
            dockerImage.push()
          }
        }
      }
    }
    }
}
