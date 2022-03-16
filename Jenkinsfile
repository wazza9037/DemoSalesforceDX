pipeline {
    agent {
        // Define agent details here
		
		docker { 
				 image 'node:16.13.1-alpine'
				 } 
    }
	environment {
               // BITBUCKET_COMMON_CREDS = credentials('jenkins-bitbucket-common-creds')
			   USERNAME='abhirupsengupta20@gmail.com'
			   SERVERKEY='bin/server.key'
			   INSTANCEURL='https://login.salesforce.com/'
			   CONSUMERKEY='3MVG9G9pzCUSkzZtnzxaz29KLNK_jrrKD_QyrSX1xP5zADVh28kD11u2IBZYwS8Qq93JAk2QG.m88.DU9fMeA'
			   CURRENTBRANCH=getbranch();
            }
    stages {
		stage('Install Salesforce CLI') {
            steps {
					script{
							"wget https://developer.salesforce.com/media/salesforce-cli/sfdx/channels/stable/sfdx-linux-x64.tar.xz"
							"mkdir ./sfdx"
							"tar xJf sfdx-linux-x64.tar.xz -C ./sfdx --strip-components 1"
							"export PATH=./sfdx/bin:$PATH"
							
							//
							}
            }
			}
		stage('Checkout source code') {
            steps {
					script{
							checkout scm
							//
							}
            }
			}
        stage('Auth of SFDC Org') {
            
            steps {
					script{
							// SFDX auth commands
							"sfdx auth:jwt:grant -u ${USERNAME} -f ${SERVERKEY} -i ${CONSUMERKEY} -r ${INSTANCEURL}"
							}
            }
        }
        stage('Validation/Deployment of Code Metadata') {
            steps {
					script{
					if(CURRENTBRANCH.contains("PR-"))
						{
						 "sfdx force:source:deploy -p force-app -u USERNAME -c"
						}
					else
						{
						 "sfdx force:source:deploy -p force-app -u USERNAME "
						}
					}
            }
        }
    }
	
}
def getbranch()
		{
			return scm.branches[0].name
		}