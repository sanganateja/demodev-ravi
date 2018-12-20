pipeline {
    agent { label 'new-jen-dind' }
	stages {
      stage('Checking out demo-common') {
        steps {
          sh 'mkdir -p demo-common'
		  dir('./demo-common-common'){
	         checkout([$class: 'GitSCM', branches: [[name: 'com-devlop']], 
			 doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], 
			 userRemoteConfigs: [[credentialsId: 'ravgit', 
			 url: 'https://github.com/sanganateja/demodev-ravi.git']]])
               echo "Building demo-common"
               sh 'mvn clean install' 
			   }
			}
        }
        stage('Checking out demo-test') {
        steps {
          sh 'mkdir -p demo-test'
		  dir('./demo-test'){
             	checkout([$class: 'GitSCM', branches: [[name: 'test-devlop']], 
				doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], 
				userRemoteConfigs: [[credentialsId: 'ravgit', 
				url: 'https://github.com/sanganateja/demodev-ravi']]])	  
                echo "Building demo-test"
               sh 'mvn clean install'
				}
            }
        }
        stage('build')
		 steps {
	       sh '''
              echo "Building the code"
               mvn package
               mvn install 
               mvn clean install
              '''       
			}
		}
        stage('TEST') {
        steps {
           sh '''
              mvn test
              '''
        }
      }
		stage('dockerbuild') {
		   steps {
		      sh 'docker build -t sangana-dev.oi/dev/mydev-image:${BUILD_NUMBER} .'
			}
	    }
        stage("push to DTR") {
		  steps{
		   withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: 'ravi-docker-cred',
		   usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD']]) {
		  	
			sh """
		        docker login --username $USERNAME --password $PASSWORD
                docker push sangana-dev.oi/dev/mydev-image:${BUILD_NUMBER}
		      """
			  }
			}
        }
	    stage('DEPLOY TO DEV ENV') {
             steps {     
			
            }
          }
        }	
    }
}	
