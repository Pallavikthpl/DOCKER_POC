pipeline {
    agent any
	environment
	{
	ant_build = "C:\\Users\\PallaviKathpalia\\IBM\\IIBT10\\workspace_New\\IIB_Build_Process\\HttpReqReply\\Build\\build.xml"
	}
    stages {
	    
        stage('Checkout') { 
            
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[url: 'https://github.com/Pallavikthpl/IIB_Build_Process.git']]])
            }
        }
        
        stage('Sonarqube') {
		environment {
        scannerHome = tool 'LocalSonarQubeScanner'
		}
		steps {
        withSonarQubeEnv('sonar_server') {
            bat "${scannerHome}/bin/sonar-scanner"
        }
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
			def server = Artifactory.server 'frogArtifactory'
			def uploadSpec = """{
			"files": [{
			"pattern": "C:/Apurva/Bars/",
			"target": "SampleRepo"
			}]
			}"""
 
			server.upload(uploadSpec)
			}
		}
		}
        stage('Deploy') {
                
            steps {
                bat "echo Deploy"
            }
        }
    }
}
